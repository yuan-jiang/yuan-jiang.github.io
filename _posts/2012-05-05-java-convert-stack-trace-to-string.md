---
layout: post
title: Java convert stack trace to string
date: 2012-05-05 22:15:00 +0800
author: Yuan Jiang
tags: java
---

Convert java stack trace to string.

{% highlight java %}
public class StackTraceReader
{
	public static String readStackTrace(Throwable e)
  {
        StringWriter stringWriter = new StringWriter();
        PrintWriter printWriter = new PrintWriter(stringWriter);
        e.printStackTrace(printWriter);
        return stringWriter.toString();
    }
}
{% endhighlight %}
