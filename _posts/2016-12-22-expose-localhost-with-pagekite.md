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
# ports that frontend is listening for local service to connect to and also for outsiders to access, comma separated, such as 80,443
ports=8080
# protocols that frontend is using for outsiders to access, such as http,https
protos=http
# domain of the frontend server and password is for backend authentication during pagekite connection
domain=http:xxx.com:password
# if dns is configured to allow for wildcard naming resolution, then using the * like below gives users free choices for kite names
domain=http:*.xxx.com:password
# if tls/ssl will be used (e.g. 443 port), then below entry is for specifying the pem file (containing certificate and key)
#tls_endpoint=xxx.com:/path/to/pem
{% endhighlight %}

## How to config pagekite.py backend with rc file and make connection to expose local services
- Create a config file at: ~/.pagekite.rc with below content
{% highlight properties %}
kitename=KITENAME
kitesecret=PASSWORD
frontend=FRONTEND_HOSTNAME:FRONTEND_PORT
{% endhighlight %}
- Run command below to expose http://localhost:8080 for example:
{% highlight bash %}
$ pagekite.py 8080 KITENAME
{% endhighlight %}
- Run command below to expose running web app on other known hosts (with ip: x.x.x.x):
{% highlight bash %}
$ pagekite.py x.x.x.x:yy KITENAME
{% endhighlight %}
- Run command below to expose a local directory and files:
{% highlight bash %}
$ pagekite.py <directory> KITENAME +indexes
{% endhighlight %}
- Run command below to expose html files:
{% highlight bash %}
$ pagekite.py *.html KITENAME
{% endhighlight %}
- NOTE: If need different KITENAMEs for above examples, multiple kitename entries should be added in ~/.pagekite.rc file:
{% highlight properties %}
kitename=KITENAME1
kitename=KITENAME2
...
kitename=KITENAMEX
kitesecret=PASSWORD
frontend=FRONTEND_HOSTNAME:FRONTEND_PORT
{% endhighlight %}

## How to connect to frontend directly from command line without using ~/.pagekite.rc
{% highlight bash %}
# expose a local service running on port 8080
## specify proto and port:
$ pagekite.py --clean --frontend=FRONTEND-HOST:FRONTEND-PORT --service_on=http/8080:KITENAME:localhost:8080:password
## specify proto and use default port frontend is listening on
$ pagekite.py --clean --frontend=FRONTEND-HOST:FRONTEND-PORT --service_on=http:KITENAME:localhost:8080:password
{% endhighlight %}

## Others
- Pagekite.py can not only be used for exposing http service, but also ssh or technically any other protocols
- Pagekite.py can also be used to quickly expose static files or directories thanks to its built web http server
- [pagekite.me](http://pagekite.me) is a paid frontend where you can register a custom name like http://youname.pagekite.me for public access
- Similar tools and services are also available, such as: [ngrok](https://ngrok.com/), [localtunnel](https://localtunnel.github.io/www/), and etc.
- Pagekite is open source and available [here](https://github.com/pagekite/PyPagekite)

## References
- [PageKite Technical Manual](https://pagekite.net/wiki/Floss/TechnicalManual/)
- [Access localhost with pagekite](http://javier.io/blog/en/2013/04/06/access-localhost-with-pagekite.html)
- [HOW TO RUN A PAGEKITE SERVER TO EXPOSE YOUR RASPBERRY PI](http://hackaday.com/2016/09/21/how-to-run-a-pagekite-server-to-expose-your-raspberry-pi/)
