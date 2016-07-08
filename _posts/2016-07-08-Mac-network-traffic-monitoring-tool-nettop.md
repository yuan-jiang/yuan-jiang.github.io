---
layout: post
title: Mac network traffic monitoring tool nettop
date: 2016-07-08 09:48:00 +0800
author: Yuan Jiang
tags: mac networking
---

Mac network traffic command line monitoring tool nettop.

Basic usage
{% highlight bash %}
$ nettop
{% endhighlight %}

Custom usage
{% highlight bash %}
$ nettop -m route
$ nettop -m tcp
$ nettop -m udp
{% endhighlight %}

Shortcut keys after nettop is open
{% highlight text %}
h: show help dialog
p: toggle readable formats (kb, mb)
d: show delta count
up & down arrows: navigate up and down in the list
right & left arrows: expand or collapse
q: quit the nettop tool
{% endhighlight %}
