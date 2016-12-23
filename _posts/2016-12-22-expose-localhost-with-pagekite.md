---
layout: post
title: Expose localhost with pagekite.py
date: 2016-12-22 16:47:00 +0800
author: Yuan Jiang
tags: networking
---

[Pagekite.py](https://pagekite.net/) is tool using reverse proxy to create a tunnel to your localhost which is then publicly accessible from outside your networks, and this is also known as localhost tunneling which is especially useful when you want to quickly test or access a local running web app behind NAT and firewall from outside of your networks and don't want to go through the complicated processes such as: buying a cloud-based server like vps for dns configuration with public static ip, or deployment to cloud based instances like aws ec2, and etc.

## Terminology
- Pagekite backend: A pagekite backend is your local running web app that is behind NAT or firewall and does not have publicly accessible static ip address and domain name. It is the one you want to be exposed to internet.
- Pagekite frontend: A pagekite frontend is a proxy server that is on the internet with a publicly accessible static ip address or domain name and to which a pagekite backend is connecting to for proxy and also through which outsiders can access for further accessing your local running web app.

## How to install pagekite.py
- Install for backend (Mac, Linux)
{% highlight bash %}
$ curl -s https://pagekite.net/pk/ |sudo bash

# After above command succeeds, pagekite.py should be available at:
/usr/local/bin/pagekite.py

# See help doc
$ pagekite.py --help
{% endhighlight %}

- Install for frontend (Linux - Ubuntu)
{% highlight bash %}
# Add our repository to /etc/apt/sources.list
$ echo deb http://pagekite.net/pk/deb/ pagekite main | sudo tee -a /etc/apt/sources.list

# Add the PageKite packaging key to your key-ring
$ sudo apt-key adv --recv-keys --keyserver keys.gnupg.net AED248B1C7B2CAC3

# Refresh your package sources by issuing
$ sudo apt-get update

# Install pagekite !
$ sudo apt-get install pagekite

# After above steps, pagekite command should be available at:
/usr/bin/pagekite
{% endhighlight %}

- Install with other methods:  
  + [Built Debian packages](http://pagekite.net/pk/deb/pool/main/p/)
  + [RPM packages & Yum](http://pagekite.net/wiki/Howto/GNULinux/RpmPackage/)
  + [Pagekite.py source file](http://pagekite.net/downloads)  

Note that installing with deb (apt) or rpm (yum) will install bonus scripts for auto start and upgrade and configs, so it should be preferred on linux (ubuntu or centos).

## How to config pagekite.py frontend
{% highlight bash %}
# Remove all lines in below file:
sudo vi /etc/pagekite.d/10_account.rc

# Edit below file:
sudo vi /etc/pagekite.d/20_frontend.rc
# Add below content:
isfrontend
# the ports are the ports that frontend are listening for outsiders to access, comma separated, such as 80,443
ports=8080
# the protos are the protocols frontend are using for outsiders to access, such as http,https
protos=http
# the domain is the domain of the frontend server and password is for backend authentication
domain=http://xxx.com:password
{% endhighlight %}

## How to config pagekite.py backend and make connection to expose local running web app
- Create a config file at: ~/.pagekite.rc with below content
{% highlight properties %}
kitename=xxx.com
kitesecret=password
frontend=xxx.com:8080
{% endhighlight %}
- Run command below to expose http://localhost:8080 for example:
{% highlight bash %}
$ pagekite.py 8080 xxx.com:8080
{% endhighlight %}

## Others
- Pagekite.py can not only be used for exposing http service, but also ssh or technically any other protocols
- Pagekite.py can also be used to quickly expose static files or directories thanks to its built web http server
- [pagekite.me](http://pagekite.me) is a paid frontend where you can register a custom name like http://youname.pagekite.me for public access
- Similar tools and services are also available, such as: [ngrok](https://ngrok.com/), [localtunnel](https://localtunnel.github.io/www/), and etc.
- Pagekite is open source and available [here](https://github.com/pagekite/PyPagekite)
