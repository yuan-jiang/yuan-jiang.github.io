---
layout: post
title: Python literal conversion
date: 2015-01-06 16:38:00 +0800
author: Yuan Jiang
tags: python
---

Python literal conversion using the "ast.literal_eval()" method.

Convert string to boolean
{% highlight ipython %}
In [1]: s = "False"

In [2]: b1 = bool(s)

In [3]: b1
Out[3]: True

In [4]: import ast

In [5]: b2 = ast.literal_eval(s)

In [6]: b2
Out[6]: False
{% endhighlight %}

Convert string to list
{% highlight ipython %}
In [1]: s = "[1, 2, 3]"

In [2]: l1 = list(s)

In [3]: l1
Out[3]: ['[', '1', ',', ' ', '2', ',', ' ', '3', ']']

In [4]: import ast

In [5]: l2 = ast.literal_eval(s)

In [6]: l2
Out[6]: [1, 2, 3]
{% endhighlight %}

Convert string to tuple
{% highlight ipython %}
In [1]: s = "(1, 2, 3)"

In [2]: t1 = tuple(s)

In [3]: t1
Out[3]: ('(', '1', ',', ' ', '2', ',', ' ', '3', ')')

In [4]: import ast

In [5]: t2 = ast.literal_eval(s)

In [6]: t2
Out[6]: (1, 2, 3)
{% endhighlight %}

Convert string to dict
{% highlight ipython %}
In [1]: s = "{'name':'andy','age':30}"

In [2]: d1 = dict(s)
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-2-b571b3cd1a9f> in <module>()
----> 1 d1 = dict(s)

ValueError: dictionary update sequence element #0 has length 1; 2 is required

In [3]: import ast

In [4]: d2 = ast.literal_eval(s)

In [5]: d2
Out[5]: {'age': 30, 'name': 'andy'}
{% endhighlight %}

Documentation of ast.literal_eval
{% highlight text %}
Safely evaluate an expression node or a string containing Python expression.  
The string or node provided may only consist of the following Python literal
structures: strings, numbers, tuples, lists, dicts, booleans, and None.
{% endhighlight %}

See [Converting from a string to boolean in Python?](http://stackoverflow.com/questions/715417/converting-from-a-string-to-boolean-in-python)
