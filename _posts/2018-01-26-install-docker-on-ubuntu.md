---
layout: post
title: Install docker on ubuntu
date: 2018-01-26 15:57:00 +0800
tags: linux
---

Command list to install docker ce on ubuntu (17.10).

{% highlight bash %}
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu zesty stable"

sudo apt-get update
sudo apt-get install docker-ce

# install docker-compose
sudo apt-get install docker-compose
{% endhighlight %}

See [reference](https://gist.github.com/levsthings/0a49bfe20b25eeadd61ff0e204f50088)