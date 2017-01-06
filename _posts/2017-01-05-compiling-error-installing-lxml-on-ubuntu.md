---
layout: post
title: Compiling error installing lxml on ubuntu
date: 2017-01-05 19:51:00 +0800
author: Yuan Jiang
tags: ubuntu python
---

A compiler error was observed during installing lxml on ubuntu the error looks something like: [error: command 'x86_64-linux-gnu-gcc' failed with exit status 4](http://stackoverflow.com/q/24455238), and it's most likely caused by low memory.

## Solutions

- Enlarge memory if possible, for example, if the issue was encountered on a virtual machine

- If enlarging memory is not possible, then add swap file:
{% highlight bash %}
sudo dd if=/dev/zero of=/swapfile bs=1024 count=524288
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
{% endhighlight %}

See [reference](http://stackoverflow.com/questions/24455238/lxml-installation-error-ubuntu-14-04-internal-compiler-error).
