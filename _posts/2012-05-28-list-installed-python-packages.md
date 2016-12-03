---
layout: post
title: List installed python packages
date: 2012-05-28 15:21:00 +0800
author: Yuan Jiang
tags: python
---

How to find out or list the installed python packages.

## Use python builtin help function
{% highlight python %}
help("modules")
{% endhighlight %}

## Use python pip tool
{% highlight bash %}
$ pip list
$ pip show
$ pip freeze
{% endhighlight %}

## References
- [python : list all packages installed](http://blog.revathskumar.com/2011/10/python-list-all-packages-installed.html)
