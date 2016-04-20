---
layout: post
title: Java convert string to stream
date: 2014-11-17 12:50:00 +0800
author: Yuan Jiang
tags: java
---

How to convert string to stream in java.

{% highlight java %}
import org.apache.commons.io.IOUtils;

InputStream is = IOUtils.toInputStream(String input);
{% endhighlight %}

See [Apache Commons IO](https://commons.apache.org/proper/commons-io/) for more.
