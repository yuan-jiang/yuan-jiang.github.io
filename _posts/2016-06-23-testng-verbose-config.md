---
layout: post
title: Testng verbose config
date: 2016-06-23 11:26:00 +0800
author: Yuan Jiang
tags: testng
---

In testng.xml, the "verbose" attribute of the suite tag is very useful for troubleshooting configuration related error. Its values can be set as 1 to 10, and smaller value means lower verbose and bigger value means higher verbose.

{% highlight xml %}
<suite name="Test suite name" verbose="1">
    <test name="Test case name" preserve-order="true">
        <packages>
            <package name="space.yuanjiang.test.*">
                ...
            </package>
        </packages>
    </test>
    ...
</suite>
{% endhighlight %}

Reference [here](http://seleniumone-by-arun.blogspot.jp/2013/05/158-understanding-usage-of-verbose.html)
