---
layout: post
title: Redirect http to https in nginx
date: 2017-01-22 17:24:00 +0800
author: Yuan Jiang
tags: nginx
---

How to redirect http traffic to https in nginx configuration.

{% highlight nginx %}
server {
    listen  80;
    server_name domain.name;
    return 301 https://$server_name$request_uri;
}

server {
    listen   443 ssl;
    ssl_certificate  domain.name.crt;
    ssl_certificate_key  domain.name.key;
    server_name  domain.name;
    location / {
        ...
    }
}
{% endhighlight %}

## References
- [Converting rewrite rules](http://nginx.org/en/docs/http/converting_rewrite_rules.html)
- [How to force or redirect to ssl in nginx](http://serverfault.com/a/424016)
