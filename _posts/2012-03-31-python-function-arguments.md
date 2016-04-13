---
layout: post
title: Python function arguments
date: 2012-03-31 15:14:00 +0800
author: Yuan Jiang
tags: python
---

Python function standard arguments, positional arguments, and keyword arguments.

## Standard arguments:
{% highlight ipython %}
In [1]: def add(x, y):
   ...:     return x + y
   ...:

In [2]: add(1, 2)
Out[2]: 3
{% endhighlight %}

## Positional arguments:
{% highlight ipython %}
In [1]: def add(*args):
   ...:     if args:
   ...:         sum = 0
   ...:         for x in args:
   ...:             sum += x
   ...:         return sum
   ...:     return None
   ...:

In [2]: add()

In [3]: add(1)
Out[3]: 1

In [4]: add(1,2)
Out[4]: 3

In [5]: add(1,2,3)
Out[5]: 6
{% endhighlight %}

## Keyword arguments:
{% highlight ipython %}
In [1]: def print_profile(**kwargs):
   ...:     for key in kwargs.keys():
   ...:         print "var=%s,value=%s" %(key, kwargs[key])
   ...:         

In [2]: print_profile()

In [3]: print_profile(name='andy',age=30,profession='engineer')
var=age,value=30
var=profession,value=engineer
var=name,value=andy
{% endhighlight %}

## Different types of arguments together:
{% highlight ipython %}
In [2]: def func(arg1, arg2, *args, **kwargs):
   ...:     print arg1
   ...:     print arg2
   ...:     print args
   ...:     print kwargs
   ...:     

In [3]: func()
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-3-08a2da4138f6> in <module>()
----> 1 func()

TypeError: func() takes at least 2 arguments (0 given)

In [4]: func(1,2)
1
2
()
{}

In [5]: func(1,2,3,4,5)
1
2
(3, 4, 5)
{}

In [6]: func(1,2,3,4,5,name='andy',age=30,profession='engineer')
1
2
(3, 4, 5)
{'age': 30, 'profession': 'engineer', 'name': 'andy'}
{% endhighlight %}

## Alternate way to pass values to positional and keyword arguments:
{% highlight ipython %}
# positional arguments: pass iterable preceded by *
In [2]: def add(*args):
   ...:        if args:
   ...:                sum = 0
   ...:                for x in args:
   ...:                    sum += x
   ...:                    return sum
   ...:        return None
   ...:

In [3]: data = (1, 2, 3)

In [4]: add(*data)
Out[4]: 6

In [5]: data = [1, 2, 3, 4]

In [6]: add(*data)
Out[6]: 10

# keyword arguments: pass dict preceded by **
In [4]: info = {'name':'andy','age':30,'profession':'engineer'}

In [5]: print_profile(info)

In [6]: print_profile(**info)
var=age,value=30
var=profession,value=engineer
var=name,value=andy
{% endhighlight %}

## References
- [Python *args and **kwargs](http://stackoverflow.com/a/3394898)
- [How to use *args and **kwargs in Python](http://www.saltycrane.com/blog/2008/01/how-to-use-args-and-kwargs-in-python/)
