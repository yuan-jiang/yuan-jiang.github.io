---
layout: post
title: Nginx to share default http port
date: 2016-12-23 17:49:00 +0800
author: Yuan Jiang
tags: nginx
---

How to use [nginx](http://nginx.org/) to proxy http request to default port (e.g. 80) to other processes running on different ports on the same server.

## Add a config file named "myconfig" under /etc/nginx/sites-available
{% highlight nginx %}
server {
    listen   80;
    server_name  domain1;
    location / {
        proxy_pass http://0.0.0.0:8080;
        include /etc/nginx/proxy_params;
    }
}

server {
    listen   80;
    server_name  domain2;
    location / {
        proxy_pass http://0.0.0.0:8081;
        include /etc/nginx/proxy_params;
    }
}
{% endhighlight %}

## Link the config file to /etc/nginx/sites-enabled
{% highlight bash %}
$ cd /etc/nginx/sites-enabled
$ ln -s /etc/nginx/sites-available/myconfig myconfig
{% endhighlight %}

## Restart nginx and you're all set
{% highlight bash %}
$ service nginx restart
{% endhighlight %}

See this stackoverflow [post](http://stackoverflow.com/a/39960487) for reference.
