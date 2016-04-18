---
layout: post
title: Python open file with with statement
date: 2014-04-01 16:05:00 +0800
author: Yuan Jiang
tags: python
---

It is good practice to use the with keyword when dealing with file objects. This has the advantage that the file is properly closed after its suite finishes, even if an exception is raised on the way. It is also much shorter than writing equivalent try-finally blocks.

{% highlight ipython %}
In [1]: with open('/tmp/test','r') as f1:
   ...:     print f1.read()
   ...:     
hello, python


In [2]: f1.closed
Out[2]: True

In [3]: f2 = open('/tmp/test', 'r')

In [4]: print f2.read()
hello, python


In [5]: f2.closed
Out[5]: False
{% endhighlight %}

Also see [PEP 343 -- The "with" Statement](https://www.python.org/dev/peps/pep-0343/)
