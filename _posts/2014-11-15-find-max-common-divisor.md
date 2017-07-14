---
layout: post
title: Find max common divisor
date: 2014-11-15 13:20:00 +0800
author: Yuan Jiang
tags: algorithm python
---

Find max common divisor.

Python examples:
{% highlight ipython %}
In [1]: def find_max_common_divisor(x, y):
   ...:     if x == y:
   ...:         return x
   ...:     else:
   ...:         small = x if x < y else y
   ...:         for i in range(1, small + 1):
   ...:             if x % i == 0:
   ...:                 if y % i == 0:
   ...:                     tmp = i
   ...:         return tmp
   ...:     

In [2]: print find_max_common_divisor(24, 36)
12
{% endhighlight %}

{% highlight ipython %}
In [1]: def gcd(a, b):
   ...:     while b:
   ...:         a, b = b, a % b
   ...:     return a
   ...:

In [2]: gcd(12, 18)
Out[2]: 6
{% endhighlight %}
