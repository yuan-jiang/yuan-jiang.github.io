---
layout: post
title: Enable ssl with letsencrypt
date: 2017-01-22 16:15:00 +0800
author: Yuan Jiang
tags: ssl
---

[Let’s Encrypt](https://letsencrypt.org/) is a free, automated, and open CA (Certificate Authority) and let's make use of it to enable ssl access to web application serviced by nginx.

## Download [certbot](https://certbot.eff.org/) tool
{% highlight bash %}
$ wget https://dl.eff.org/certbot-auto
$ chmod a+x certbot-auto
$ ./certbox-auto
{% endhighlight %}

## Get certificate for your domain name
{% highlight bash %}
$ ./certbot-auto certonly --nginx -d <domain.name>
{% endhighlight %}

## Find and verify that privkey.pem and fullchain.pem exists
{% highlight bash %}
/etc/letsencrypt/live/<domain.name>/privkey.pem
/etc/letsencrypt/live/<domain.name>/fullchain.pem
{% endhighlight %}

## Config on nginx
{% highlight nginx %}
server {
    listen   80;
    listen   443 ssl;
    server_name  domain.name;
    ssl_certificate  /etc/letsencrypt/live/domain.name/fullchain.pem;
    ssl_certificate_key  /etc/letsencrypt/live/domain.name/privkey.pem;
    location / {
        proxy_pass http://0.0.0.0:8080;
        include /etc/nginx/proxy_params;
    }
}
{% endhighlight %}

## Renew or update certificate
{% highlight bash %}
./certbot-auto renew
{% endhighlight %}

## References
- [Nginx SSL Certificate Errors](https://ma.ttias.be/nginx-ssl-certificate-errors-pem_read_bio_x509_aux-pem_read_bio_x509-ssl_ctx_use_privatekey_file/)
- [Configuring HTTPS servers](http://nginx.org/en/docs/http/configuring_https_servers.html)
- [Certbot](https://certbot.eff.org/docs/install.html#certbot-auto)
