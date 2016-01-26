---
layout: post
title: Linux useful commands and how-to collections
date: 2014-11-22 09:00:00 +0800
author: Yuan Jiang
tags: linux
---

Linux useful commands and how-to collections.

Config for mount with nfs
{% highlight bash %}
1) set on source:
$ vi /etc/exports
add below entry:
=> /path/to/share *(ro, root_squash)
$ service nfs start
2) set on destination:
$ mount source_ip:/path/to/share /destination/dir
{% endhighlight %}

Run google chrome as root
{% highlight bash %}
$ vi /usr/bin google-chrome
=> add "--user-data-dir" at the very end of the file
=> e.g. exec -a "$0" "$HERE/chrome" "$@" --user-data-dir
{% endhighlight %}

Install google-chrome-stable on ubuntu
{% highlight bash %}
$ wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
$ sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
$ sudo apt-get update sudo apt-get install google-chrome-stable
{% endhighlight %}

Zip all files under directory and unzip to directory
{% highlight bash %}
$ zip -r file.zip directory
$ unzip -d destination file.zip
{% endhighlight %}

Test HTTP using telnet
{% highlight bash %}
$ telnet www.google.com 80
=> This will connect to www.google.com on port 80
GET /
GET /index.html HTTP/1.1
GET /index.html HTTP/1.1 host:www.google.com
=> Hit the <enter> twice
=> This will output the info of http response
{% endhighlight %}

Install lxml on ubuntu
{% highlight bash %}
$ sudo apt-get install libxml2-dev libxslt1-dev python-dev
=> For older version:
$ sudo apt-get install python-lxml
{% endhighlight %}

Config default java on rhel
{% highlight bash %}
$ /usr/sbin/alternatives --config java
{% endhighlight %}

Force a clock update using ntp
{% highlight bash %}
$ sudo ntpdate -s time.nist.gov
{% endhighlight %}

Find ip addresses
{% highlight bash %}
$ ifconfig | grep inet | grep -v inet6 | grep -v 127.0.0.1 | awk '{print $2}' | sed 's/addr://'
{% endhighlight %}

Add private key for ssh connection to automatically load and use
{% highlight bash %}
$ ssh-agent `ssh-add ~/.ssh/custom-key`
{% endhighlight %}

DNS query
{% highlight bash %}
$ nslookup <target>
$ host <target>
$ dig <target>
{% endhighlight %}

Count number of files
{% highlight bash %}
$ ls -l <dir> | wc -l
{% endhighlight %}

Replace string in files using sed
{% highlight bash %}
$ sed -i -e 's/old/new/g' /path/to/file
{% endhighlight %}

Get full path of a file
{% highlight bash %}
$ readlink -f file.txt
{% endhighlight %}

Extract substring
{% highlight bash %}
sub=${parent:start:length}
=> a="helloworld"
=> b=${a:1:3}
=> echo $b
=> ell
{% endhighlight %}

Open file on command line using default editor
{% highlight bash %}
=> if desktop is gnome:
$ gnome-open file
=> if desktop is kde:
$ kde-open file
or
$ xdg-open file
{% endhighlight %}

Run command until success
{% highlight bash %}
until command-succeeds do
    another-command
done
{% endhighlight %}

Get yesterday date
{% highlight bash %}
$ date -d "yesterday 13:00 " '+%Y-%m-%d'
{% endhighlight %}

Shortcuts of input prompt
{% highlight bash %}
CTRL-U: Delete everything before cursor
CTRL-K: Delete everything after cursor
CTRL-Y: Paste the deleted input
CTRL-E: Jump to the end of input
{% endhighlight %}

Start jenkins in standalone mode
{% highlight bash %}
$ java -DHUDSON_HOME=dir -jar jenkins.war --httpPort=80
{% endhighlight %}

One-line alert with python
{% highlight bash %}
$ python -c "import Tkinter, tkMessageBox; Tkinter.Tk().wm_withdraw(); tkMessageBox.showinfo('Tips', 'Hi there it\'s time to have a little rest')"
{% endhighlight %}

Show linux kernel info and distro
{% highlight bash %}
$ cat /proc/sys/kernel/osrelease
$ lsb_release -a
$ uname -a
{% endhighlight %}

Execute two commands
{% highlight bash %}
$ cmd1 && cmd2
=> cmd2 will execute only if cmd1 succeeds (return exit code 0)
$ cmd1 ;  cmd2
=> cmd2 will execute regardless cmd1 success or failure
{% endhighlight %}

Run program in background
{% highlight bash %}
$ nohup java -jar app.jar &
{% endhighlight %}

Find application paths
{% highlight bash %}
$ find / -name <appname>
{% endhighlight %}

Change computer name
{% highlight bash %}
$ vi /etc/hostname
$ vi /etc/hosts
{% endhighlight %}

Set new date
{% highlight bash %}
$ date --set="STRING"
$ date -s "STRING"
e.g.
$ date --set="22 Nov 2014 09:00:00"
{% endhighlight %}

Set prompt character
{% highlight bash %}
$ vi ~/.bashrc
=> PS1="[\u@\h \W]\\$ "
=> PS1="[\u@\h \W]# "
{% endhighlight %}

List installed packages on rhel and centos
{% highlight bash %}
=> rpm -qa
=> yum list installed
{% endhighlight %}

Config static ip address for ubuntu
{% highlight bash %}
1) Add hostname to /etc/hostname
2) Set dns in /etc/resolv.config
=> nameserver xx.xx.xx.xx
3) Set static ip address in /etc/network/interfaces
auto eth0
iface eth0 inet static
address xxx.xxx.xxx.xxx
netmask 255.255.255.0
gateway xxx.xxx.xxx.xxx
{% endhighlight %}

Check http resource but not download
{% highlight bash %}
$ wget --spider url
$ curl -I url
$ curl --head url
{% endhighlight %}

Check disk info
{% highlight bash %}
=> List free disk space
$ df -h or df -k
=> List space usage of file or directory
$ du -sh
{% endhighlight %}

Route table
{% highlight bash %}
$ route -n
=> View routing table
$ route add -net xx.xx.xx.xx netmask 255.255.255.0 gw xx.xx.xx.xx
=> Add route
$ route del -net xx.xx.xx.xx netmask 255.255.255.0 gw xx.xx.xx.xx
=> Delete route
{% endhighlight %}

Write to syslog
{% highlight bash %}
$ logger hello
{% endhighlight %}

Display system uptime
{% highlight bash %}
$ uptime
{% endhighlight %}

Display users and activity
{% highlight bash %}
$ w
$ users
$ who
$ whoami
$ last
{% endhighlight %}

List open files
{% highlight bash %}
$ lsof
$ lsof -i:22
$ lsof -u ubuntu
{% endhighlight %}

List most appearing of english words in text
{% highlight bash %}
cat xxx-file | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -n 10
{% endhighlight %}
