---
layout: post
title: Python decorator with class
date: 2017-08-18 15:51:00 +0800
tags: python aop
---

Python decorators are usually created with function, see another related [post](/python-aop-with-decorators), but this post also shows an example on how to create decorators with class.

{% highlight ipython %}
In [1]: def greet(name):
   ...:     print("Hi %s" % name)
   ...:

In [2]: greet("Andy")
Hi Andy

In [3]: class Polite(object):
   ...:     def __init__(self, func):
   ...:         self.func = func
   ...:     def __call__(self, *args, **kwargs):
   ...:         self.func(*args, **kwargs)
   ...:         print("Nice to meet you")
   ...:

In [4]: @Polite
   ...: def greet(name):
   ...:     print("Hi %s" % name)
   ...:

In [5]: greet("Andy")
Hi Andy
Nice to meet you
{% endhighlight %}