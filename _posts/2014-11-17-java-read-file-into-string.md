---
layout: post
title: Java read file into string
date: 2014-11-17 11:11:00 +0800
author: Yuan Jiang
tags: java
---

How to read content of a file into string in java.

{% highlight java %}
import org.apache.commons.io.IOUtils;

String s = IOUtils.toString(new FileInputStream("/tmp/file"));
{% endhighlight %}

See [Apache Commons IO](https://commons.apache.org/proper/commons-io/) for more.
