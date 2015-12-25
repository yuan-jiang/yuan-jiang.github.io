---
layout: post
title: Config postfix with gmail to send email from linux command line
date: 2013-12-25 10:55:00 +0800
author: Yuan Jiang
tags: linux
---

A working example on how to config postfix with gmail smtp server to send email directly from linux command line and tested on vagrant vm with ubuntu precise 32.

Install necessary packages
{% highlight bash %}
$ sudo apt-get install postfix mailutils libsasl2-2 ca-certificates libsasl2-modules
{% endhighlight %}

Open postfix config file /etc/postfix/main.cf and add below content
{% highlight bash %}
relayhost = [smtp.gmail.com]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/postfix/cacert.pem
smtp_use_tls = yes
{% endhighlight %}

Open /etc/postfix/sasl_passwd and add below line
{% highlight bash %}
[smtp.gmail.com]:587    USERNAME@gmail.com:PASSWORD
=> USERNAME: your gmail username
=> PASSWORD: your gmail password or app password if 2-factor authentication is enabled
{% endhighlight %}

Set permission and update postfix config to use the sasl_passwd file
{% highlight bash %}
$ sudo chmod 400 /etc/postfix/sasl_passwd
$ sudo postmap /etc/postfix/sasl_passwd
{% endhighlight %}

Validate certificate
{% highlight bash %}
$ cat /etc/ssl/certs/Thawte_Premium_Server_CA.pem | sudo tee -a /etc/postfix/cacert.pem
{% endhighlight %}

Start postfix and reload the config
{% highlight bash %}
sudo /etc/init.d/postfix start
sudo /etc/init.d/postfix reload
or
sudo /etc/init.d/postfix restart
{% endhighlight %}

Finally send a test email
{% highlight bash %}
$ echo "Test mail from postfix" | mail -s "Test Postfix" username@anothermail.com
=> The echo message is the mail body
=> The -s message is the subject
=> The mail address at the end is the recipient
{% endhighlight %}
