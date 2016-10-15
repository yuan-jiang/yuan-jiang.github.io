---
layout: post
title: How to config shadowsocks server
date: 2015-07-26 16:40:00 +0800
author: Yuan Jiang
tags: linux
---

How to config shadowsocks server on ubuntu.

Install required libs:
{% highlight bash %}
sudo apt-get install python-setuptools, pip
sudo apt-get install python-m2crypto, supervisor
{% endhighlight %}

Install shadowsocks:
{% highlight bash %}
sudo pip install shadowsocks
{% endhighlight %}

Config shadowsocks:
{% highlight python %}
# sudo vi /etc/shadowsocks.json and add below content:
{ 
  "server": "0.0.0.0", 
  "server_port": 8388, 
  "local_port": 1080, 
  "password": "yourpassword", 
  "timeout": 600, 
  "method": "aes-256-cfb" 
}
{% endhighlight %}

Config supervisord to manage shadowsocks:
{% highlight ini %}
# sudo vi /etc/supervisor/supervisord.conf 
[program:shadowsocks] 
command=ssserver -c /etc/shadowsocks.json 
autostart=true 
autorestart=true 
user=root 
log_stderr=true 
logfile=/var/log/shadowsocks.log
{% endhighlight %}

Start supervisor:
{% highlight bash %}
sudo /etc/init.d/supervisor start
{% endhighlight %}

[Optional] Config autostart after system restart:
{% highlight text %}
Add above start command to /etc/rc.local
{% endhighlight %}
