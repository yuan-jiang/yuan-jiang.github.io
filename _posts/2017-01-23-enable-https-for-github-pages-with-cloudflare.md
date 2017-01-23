---
layout: post
title: Enable https for github pages with cloudflare
date: 2017-01-23 13:57:00 +0800
author: Yuan Jiang
tags: ssl
---

Github pages currently does not support https for blogs that use custom domain name, and here is what [CloudFlare](https://www.cloudflare.com/) comes to a rescue.

## Simple steps and instructions
- Register cloudflare account and add your custom domain;
- Go to domain registrar site and change dns servers to what cloudflare provides;
- Migrate dns resolving records to cloudflare (from previous/old domain registrar site);
- Ensure ssl setting is set to "Flexible SSL" on cloudflare setting;

## Post-procedures

- Modify github pages "_config.yml" (e.g. jekyll powered)
{% highlight yaml %}
url: https://your-domain-name   # with the https protocol
enforce_ssl: your-domain-name   # without any protocol
{% endhighlight %}

- Create cloudflare page rules to redirect http to https
{% highlight text %}
$ http://*your-domain-name/*
$ => always use ssl
{% endhighlight %}

- Update static resources (css/js/html/image/etc) to use https url

## References
- [Set Up SSL on Github Pages With Custom Domains for Free](https://sheharyar.me/blog/free-ssl-for-github-pages-with-custom-domains/)
- [How to host your static site with HTTPS on GitHub Pages and CloudFlare](https://developer.ubuntu.com/en/blog/2016/02/17/how-host-your-static-site-https-github-pages-and-cloudflare/)
