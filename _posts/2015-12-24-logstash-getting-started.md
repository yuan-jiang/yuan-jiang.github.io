---
layout: post
title: Logstash getting started
date: 2015-12-24 16:10:00 +0800
author: Yuan Jiang
tags: devops
---

Logstash getting started simple guide covering installation and a simple example.

Environment
{% highlight bash %}
Fresh vagrant vm with ubuntu precise32
{% endhighlight %}

Ensure java 7 is installed
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install openjdk-7-jre-headless
{% endhighlight %}

Download latest logstash package
{% highlight bash %}
$ wget https://download.elastic.co/logstash/logstash/logstash-all-plugins-2.1.0.zip
{% endhighlight %}

Unzip logstash
{% highlight bash %}
$ sudo apt-get install unzip
$ unzip logstash-all-plugins-2.1.0.zip
{% endhighlight %}

Prepare a simple example conf file
{% highlight bash %}
$ cd logstash-2.1.0/
$ mkdir conf
$ cd conf && vi syslogtest.conf
{% endhighlight %}

Add below content to the above file
{% highlight ruby %}
input {
    syslog {
       type => syslog
       port => 514
    }
}

output {
   stdout {
      codec => rubydebug
   }
}
{% endhighlight %}

Start logstash with above conf file
{% highlight bash %}
$ sudo su
=> need root access for port binding
$ ../bin/logstash -f syslogtest.conf
=> if it works well, the console should output:
Settings: Default filter workers: 1
Logstash startup completed
{% endhighlight %}

Start another terminal and test with telnet on the same vm
{% highlight bash %}
vagrant@precise32:~$ telnet localhost 514
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
hello
{% endhighlight %}

Go back the previous terminal window that has logstash started which should output
{% highlight ruby %}
{
           "message" => "hello\r\n",
          "@version" => "1",
        "@timestamp" => "2015-12-24T08:05:16.272Z",
              "type" => "syslog",
              "host" => "127.0.0.1",
              "tags" => [
        [0] "_grokparsefailure_sysloginput"
    ],
          "priority" => 0,
          "severity" => 0,
          "facility" => 0,
    "facility_label" => "kernel",
    "severity_label" => "Emergency"
}
{% endhighlight %}

Start logstash as daemon you can try with below command:
{% highlight bash %}
$ nohup ./bin/logstash -f conf/syslogtest.conf &
{% endhighlight %}
However in production env it's recommended to install logstash with the official package on different flavor of linux such as RPM or Debian package, and then start logstash as service:
{% highlight bash %}
$ service logstash start
{% endhighlight %}
This is mentioned in a discussion thread of the logstash support community [here](https://discuss.elastic.co/t/how-to-start-ls1-5-as-a-background-task/33814)

Quick reference links

 - [logstash input plugins](https://www.elastic.co/guide/en/logstash/current/input-plugins.html)
 - [logstash output plugins](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)
 - [logstash filter plugins](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)
 - [logstash codec plugins](https://www.elastic.co/guide/en/logstash/current/codec-plugins.html)
