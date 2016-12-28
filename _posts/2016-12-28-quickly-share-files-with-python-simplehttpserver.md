---
layout: post
title: Quickly share files with python SimpleHTTPServer
date: 2016-12-28 14:58:00 +0800
author: Yuan Jiang
tags: python
---

With python builtin SimpleHTTPServer module, you can start an http service to easily and quickly share a directory with files.

## With default port 8000
{% highlight bash %}
$ cd $directory
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
{% endhighlight %}

## With custom port xxxx
{% highlight bash %}
$ cd $directory
$ python -m SimpleHTTPServer xxxx
Serving HTTP on 0.0.0.0 port xxxx ...
{% endhighlight %}

See python doc for [reference](https://docs.python.org/2/library/simplehttpserver.html)
