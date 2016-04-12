---
layout: post
title: Python aop with decorators
date: 2012-03-30 15:55:00 +0800
author: Yuan Jiang
tags: python aop
---

Python AOP (aspect oriented programming) with decorators.

{% highlight python %}
#!/usr/bin/env python
# coding=utf-8


import time


# Original method to say hi
def say_hi(name):
    print "Hi %s!" % name


# Now we define a decorator to also say good morning/afternoon/evening
def polite(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        hour = time.localtime().tm_hour
        if hour in range(12):
            print "Good morning!"
        elif hour in range(12, 18):
            print "Good afternoon!"
        else:
            print "Good evening!"
    return wrapper


# print "Without decoration:"
# say_hi("Andy")
# print say_hi.__name__
#
# print "With decoration:"
# say_hi = polite(say_hi)
# say_hi("Andy")
# print say_hi.__name__
#
# # output:
# Without decoration:
# Hi Andy!
# say_hi
# With decoration:
# Hi Andy!
# Good afternoon!
# wrapper


# In python, the @ character can be used to simplify the func = decorator(func)
@polite
def say_hello(name):
    print "Hello %s!" % name

# print "With python @ character:"
# say_hello("Andy")
#
# # output:
# With python @ character:
# Hello Andy!
# Good afternoon!


# One issue using decorators is that the original function name will be replaced
# with the inner function name (wrapper in the above example) making it harder
# for debugging in some cases and functools.wraps is the solution to this issue.
from functools import wraps

def polite_wraps(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        hour = time.localtime().tm_hour
        if hour in range(12):
            print "Good morning!"
        elif hour in range(12, 18):
            print "Good afternoon!"
        else:
            print "Good evening!"
    return wrapper

# print "Without @wraps:"
# say_hi1 = polite(say_hi)
# say_hi1("Andy")
# print say_hi1.__name__
#
# print "With @wraps:"
# say_hi2 = polite_wraps(say_hi)
# say_hi2("Andy")
# print say_hi2.__name__
#
# # output:
# Without @wraps:
# Hi Andy!
# Good afternoon!
# wrapper
# With @wraps:
# Hi Andy!
# Good afternoon!
# say_hi

{% endhighlight %}

## References
- [PEP 318 -- Decorators for Functions and Methods](https://www.python.org/dev/peps/pep-0318/)
- [Python functools.wraps](https://docs.python.org/2/library/functools.html#functools.wraps)
- [Python decorators and aop](http://www.cnblogs.com/huxi/archive/2011/03/01/1967600.html)
- [Aspect Oriented Programming in Python using Decorators](http://blog.drorhelper.com/2010/12/aspect-oriented-programming-in-python.html)
- [Why to use @wraps with decorators](https://artemrudenko.wordpress.com/2013/04/15/python-why-you-need-to-use-wraps-with-decorators/)
