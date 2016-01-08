---
layout: post
title: Java scp util
author: Yuan Jiang
date: 2012-06-02 08:35:00 +0800
tags: java
---

Java scp util implemented with JSch lib.

{% highlight java %}
import java.io.*;
import com.jcraft.jsch.*;
import org.apache.log4j.Logger;

/**
 * @author yuanjiang
 *
 */
public class SCPUtil
{

    private static final Logger LOG = Logger.getLogger(SCPUtil.class);

    private String host = null;
    private String username = null;
    private String password = null;
    private String privateKey = null;
    private String privateKeyPass = null;

    private FileOutputStream fos = null;
    private FileInputStream fis = null;

    /**
     * SCPUtil with password-based auth
     * @param host the host to scp file from or to
     * @param username the ssh auth based username
     * @param password the ssh auth based password
     */
    public SCPUtil(String host, String username, String password)
    {
        this.host = host;
        this.username = username;
        this.password = password;
    }

    /**
     * SCPUtil with private-key-based auth
     * @param host the host to scp file from or to
     * @param username the ssh auth based username
     * @param privateKey the ssh auth based private key
     * @param privateKeyPass the ssh auth based private key passphrase
     */
    public SCPUtil(String host, String username, String privateKey, String privateKeyPass)
    {
        this.host = host;
        this.username = username;
        this.privateKey = privateKey;
        this.privateKeyPass = privateKeyPass;
    }

    /**
     * Get authenticated session
     * @return authenticated Session instance
     */
    private Session getAuthSession() throws Exception
    {
        Session session;
        JSch jsch = new JSch();
        if (this.password == null)
        {
            if (this.privateKeyPass != null)
            {
                jsch.addIdentity(this.privateKey, this.privateKeyPass);
            }
            else
            {
                jsch.addIdentity(this.privateKey);
            }
            session = jsch.getSession(this.username, this.host);
            LOG.info("Starting private key auth scp session to host: " + this.host);
        }
        else
        {
            session = jsch.getSession(this.username, this.host);
            session.setPassword(this.password);
            LOG.info("Starting password auth scp session to host: " + this.host);
        }

        java.util.Properties config = new java.util.Properties();
        config.put("StrictHostKeyChecking", "no");
        session.setConfig(config);

        return session;
    }

    /**
     * Scp from remote file to local file
     * @param remoteFile Remote file name to retrieve, including path
     * @param localFile Local file name to save, including path
     * @throws Exception
     */
    public void get(String remoteFile, String localFile)
    {

        try {
            Session session = this.getAuthSession();
            session.connect();

            LOG.info("Getting remote file: " + remoteFile + " and saving as: " + localFile);

            String command = "scp -f " + remoteFile;
            Channel channel = session.openChannel("exec");
            ((ChannelExec)channel).setCommand(command);

            OutputStream out = channel.getOutputStream();
            InputStream in = channel.getInputStream();

            channel.connect();

            byte[] buf = new byte[1024];

            buf[0] = 0;
            out.write(buf, 0, 1);
            out.flush();

            while(true)
            {
                int c = checkAck(in);
                if(c != 'C')
                {
                    break;
                }

                // read '0644 '
                in.read(buf, 0, 5);

                long filesize = 0L;
                while(true)
                {
                    if(in.read(buf, 0, 1) < 0)
                    {
                        // error
                        break;
                    }
                    if(buf[0] == ' ') break;
                    filesize = filesize * 10L + (buf[0] - '0');
                }

                String file = null;
                for(int i=0; ; i++)
                {
                    in.read(buf, i, 1);
                    if(buf[i] == (byte)0x0a)
                    {
                        file = new String(buf, 0, i);
                        break;
                    }
                }

                LOG.info("filesize=" + filesize + ", file=" + file);

                // send '\0'
                buf[0] = 0;
                out.write(buf, 0, 1);
                out.flush();

                // read a content of local file
                fos = new FileOutputStream(localFile);
                int foo;
                while(true)
                {
                    if(buf.length < filesize) foo = buf.length;
                    else foo = (int)filesize;
                    foo = in.read(buf, 0, foo);
                    if(foo < 0)
                    {
                        // error
                        break;
                    }
                    fos.write(buf, 0, foo);
                    filesize -= foo;
                    if(filesize == 0L) break;
                }
                fos.close();
                fos = null;

                if(checkAck(in) != 0)
                {
                    System.exit(0);
                }

                // send '\0'
                buf[0] = 0;
                out.write(buf, 0, 1);
                out.flush();
            }

            LOG.info("File downloaded successfully!");

            channel.disconnect();
            session.disconnect();

        }
        catch (Exception e)
        {
            LOG.error(e.getMessage(), e);
            try
            {
                if(fos != null) fos.close();
            }
            catch(Exception ee)
            {
                LOG.error(e.getMessage(), ee);
            }
        }
    }


    /**
     * Scp from local file to remote file
     * @param remoteFile Remote file name to save, including path
     * @param localFile Local file name to scp, including path
     * @throws Exception
     */
    public void put(String localFile, String remoteFile)
    {
        try {
            Session session = this.getAuthSession();
            session.connect();

            LOG.info("Putting local file: " + localFile + " to remote and saving as: " + remoteFile);

            boolean ptimestamp = true;

            // exec 'scp -t rfile' remotely
            String command = "scp " + (ptimestamp ? "-p" :"") + " -t " + remoteFile;
            Channel channel = session.openChannel("exec");
            ((ChannelExec)channel).setCommand(command);

            // get I/O streams for remote scp
            OutputStream out = channel.getOutputStream();
            InputStream in = channel.getInputStream();

            channel.connect();

            if(checkAck(in) != 0)
            {
                System.exit(0);
            }

            File lfile = new File(localFile);

            if(ptimestamp)
            {
                command = "T " + (lfile.lastModified() / 1000) + " 0";
                // The access time should be sent here,
                // but it is not accessible with JavaAPI ;-<
                command += (" " + (lfile.lastModified() / 1000) + " 0\n");
                out.write(command.getBytes());
                out.flush();
                if(checkAck(in) != 0)
                {
                    System.exit(0);
                }
            }

            // send "C0644 filesize filename", where filename should not include '/'
            long filesize = lfile.length();
            command = "C0644 " + filesize + " ";
            if(localFile.lastIndexOf('/') > 0)
            {
                command += localFile.substring(localFile.lastIndexOf('/') + 1);
            }
            else
            {
                command += localFile;
            }

            command += "\n";
            out.write(command.getBytes());
            out.flush();

            if(checkAck(in) != 0)
            {
                System.exit(0);
            }

            // send a content of lfile
            fis = new FileInputStream(localFile);
            byte[] buf = new byte[1024];
            while(true)
            {
                int len = fis.read(buf, 0, buf.length);
                if(len <= 0) break;
                out.write(buf, 0, len); //out.flush();
            }
            fis.close();
            fis = null;
            // send '\0'
            buf[0] = 0;
            out.write(buf, 0, 1);
            out.flush();
            if(checkAck(in) != 0)
            {
                System.exit(0);
            }
            out.close();


            LOG.info("filesize=" + filesize + ", file=" + remoteFile);
            LOG.info("File uploaded successfully!");

            channel.disconnect();
            session.disconnect();


        }
        catch (Exception e)
        {
            LOG.error(e.getMessage(), e);
            try
            {
                if(fis != null) fis.close();
            }
            catch(Exception ee)
            {
                LOG.error(e.getMessage(), ee);
            }
        }
    }

    static int checkAck(InputStream in) throws IOException
    {
        int b = in.read();
        // b may be 0 for success,
        //          1 for error,
        //          2 for fatal error,
        //          -1
        if(b == 0) return b;
        if(b == -1) return b;

        if(b == 1 || b == 2)
        {
            StringBuffer sb = new StringBuffer();
            int c;
            do
            {
                c = in.read();
                sb.append((char)c);
            }
            while(c != '\n');
            if(b == 1)
            { // error
                LOG.error(sb.toString());
            }
            if(b == 2)
            { // fatal error
                LOG.error(sb.toString());
            }
        }
        return b;
    }
{% endhighlight %}

Reference:  
 - [ScpFrom.java](http://www.jcraft.com/jsch/examples/ScpFrom.java.html)  
 - [ScpTo.java](http://www.jcraft.com/jsch/examples/ScpTo.java.html)
