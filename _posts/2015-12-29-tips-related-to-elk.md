---
layout: post
title: Tips related to ELK
author: Yuan Jiang
date: 2015-12-29 14:45:00 +0800
tags: devops
---

Tips related to logstash, elasticsearch and kibana.

- Permission issue about accessing sensitive files such as syslog with logstash file input:
{% highlight bash %}
$ usermod -a -G adm logstash
=> add logstash user to the adm group
{% endhighlight %}
or set acl entry like below:
{% highlight bash %}
$ setfacl -m u:logstash:r /var/log/syslog
=> add read access to logstash user for syslog file
{% endhighlight %}


#Reference Links  
- Good post series on [monitoring everything](https://ianunruh.com/2014/05/monitor-everything.html)  
- Tutorial on [How To Install Elasticsearch, Logstash, and Kibana (ELK Stack) on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04)
