---
layout: post
title: Logstash send email alert
date: 2015-12-25 11:55:00 +0800
author: Yuan Jiang
tags: devops
---

How to config logstash email output plugin to send email alert.

Ensure postfix or sendmail is configured on the logstash server, refer to another post [here](/config-postfix-with-gmail-to-send-email-from-linux-command-line) which config postfix with gmail smtp to send email

Create a new logstash conf file in /etc/logstash/conf.d and add below content
{% highlight json %}
input {
   syslog {
        type => syslog
        port => 5514
      }
    }

output {
   email {
        to => "username@example.com"
        subject => "Test from logstash email output"
        body => "The message from logstash input: %{message}"
      }
    }
{% endhighlight %}

Next restart logstash
{% highlight bash %}
$ service logstash restart
{% endhighlight %}

Finally test with telnet in another terminal on the same vm
{% highlight bash %}
$ telnet localhost 5514
$ => then after connected, enter some message and press enter
{% endhighlight %}

In a few seconds the actual "username@example.com" should receive an email sent by logstash with the subject and body matching what's configured above.
