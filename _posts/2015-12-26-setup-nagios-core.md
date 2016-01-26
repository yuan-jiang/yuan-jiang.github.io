---
layout: post
title: Setup nagios core
date: 2015-12-26 11:23:00 +0800
author: Yuan Jiang
tags: devops service-assurance service-monitoring nagios
---

Setup nagios core on ubuntu 12.04 (precise 32) with root user.

Instal LAMP
{% highlight bash %}
$ apt-get update
$ apt-get install apache2
$ apt-get install mysql-server php5-mysql
$ mysql_install_db
$ mysql_secure_installation
$ apt-get install php5 libapache2-mod-php5 php5-mcrypt
{% endhighlight %}

Create nagios user and group
{% highlight bash %}
$ useradd nagios
$ groupadd nagcmd
$ usermod -a -G nagcmd nagios
{% endhighlight %}

Install dependencies
{% highlight bash %}
$ apt-get update
$ apt-get install build-essential libgd2-xpm-dev openssl libssl-dev xinetd apache2-utils unzip
{% endhighlight %}

Download and install nagios core
{% highlight bash %}
$ cd ~
$ wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.1.1.tar.gz
$ tar xvf nagios-*.tar.gz
$ cd nagios-*
$ ./configure --with-nagios-group=nagios --with-command-group=nagcmd
$ make all
$ make install
$ make install-commandmode
$ make install-init
$ make install-config
$ /usr/bin/install -c -m 644 sample-config/httpd.conf /etc/apache2/sites-available/nagios.conf
$ usermod -G nagcmd www-data
{% endhighlight %}

Download and install nagios plugins
{% highlight bash %}
$ cd ~
$ wget http://www.nagios-plugins.org/download/nagios-plugins-2.1.1.tar.gz
$ tar xvf nagios-plugins-*.tar.gz
$ cd nagios-plugins-*
$ ./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-openssl
$ make
$ make install
{% endhighlight %}

Config nagios
{% highlight bash %}
$ vi /usr/local/nagios/etc/nagios.cfg
$ => find "cfg_dir" and uncomment below line:
$ cfg_dir=/usr/local/nagios/etc/servers
$ => then create the directory
$ mkdir /usr/local/nagios/etc/servers
$ => ensure below lines are uncommented:
$ resource_file=/usr/local/nagios/etc/resource.cfg
$ check_external_commands=1
{% endhighlight %}

Config nagios contacts
{% highlight bash %}
$ vi /usr/local/nagios/etc/objects/contacts.cfg
$ => find email directive and replace its value with a real one
$ email    nagios@localhost  ;
{% endhighlight %}

Config nagios web interface access
{% highlight bash %}
$ a2enmod rewrite
$ a2enmod cgi
$ a2enmod version
$ htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
$ ln -s /etc/apache2/sites-available/nagios.conf /etc/apache2/sites-enabled/
$ service nagios start
$ service apache2 restart
$ ln -s /etc/init.d/nagios /etc/rcS.d/S99nagios
$ service nagios restart
$ service apache2 restart
{% endhighlight %}

Open nagios in web browser
{% highlight bash %}
$ => now access nagios web
$ open http://nagios_server_public_ip/nagios
$ => user/password: nagiosadmin/[your-password-set-above]
{% endhighlight %}


##Reference links
- [Nagios core installation guide](https://assets.nagios.com/downloads/nagioscore/docs/Installing_Nagios_Core_From_Source.pdf)
- [How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 14.04](How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 14.04)
- [How To Install Nagios 4 and Monitor Your Servers on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-nagios-4-and-monitor-your-servers-on-ubuntu-14-04)
