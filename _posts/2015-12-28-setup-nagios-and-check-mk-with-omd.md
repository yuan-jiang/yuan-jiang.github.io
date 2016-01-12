---
layout: post
title: Setup nagios and check_mk with omd
author: Yuan Jiang
date: 2015-12-28 17:00:00 +0800
tags: devops
---

A few reference links on how to setup nagios with check_mk plugin with omd package.

#check_mk official:   
 - [http://mathias-kettner.com/index.html](http://mathias-kettner.com/index.html)

#check_mk download:  
 - [http://mathias-kettner.com/download/](http://mathias-kettner.com/download/)  
 - [http://mathias-kettner.com/check_mk_download_archive.php](http://mathias-kettner.com/check_mk_download_archive.php)  

#check_mk doc:  
 - [http://mathias-kettner.com/checkmk_wato.html](http://mathias-kettner.com/checkmk_wato.html)  
 - [http://mathias-kettner.com/checkmk_install_with_omd.html](http://mathias-kettner.com/checkmk_install_with_omd.html)  

#omd:  
 - [http://omdistro.org/start](http://omdistro.org/start)

#Setup with ubuntu precise32
{% highlight bash %}
$ wget http://files.omdistro.org/releases/debian_ubuntu/omd-1.20.precise.i386.deb
$ dpkg -i omd-1.20.precise.i386.deb
$ apt-get -f install
$ dpkg -i omd-1.20.precise.i386.deb
$ omd create test
$ omd start test
=> nagios with check_mk is ready
$ wget http://mathias-kettner.de/download/check-mk-agent_1.2.4p5-2_all.deb
$ dpkg -i check-mk-agent_1.2.4p5-2_all.deb
=> check_mk agent is now ready
$ http://<host-of-nagios-omd>/test
=> open browser to above url and login with default username/password: omdadmin/omd
{% endhighlight %}

#Reference links
 - Example on [How To Use Open Monitoring Distribution with Check_MK on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-use-open-monitoring-distribution-with-check_mk-on-ubuntu-14-04)
 - Example on [How To Install Nagios 4 and Monitor Your Servers on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-nagios-4-and-monitor-your-servers-on-ubuntu-14-04)
