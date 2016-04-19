---
layout: post
title: Python remove duplicate dict in list
date: 2014-11-21 11:57:00 +0800
author: Yuan Jiang
tags: python
---

Remove duplicate dict from a list in python.

{% highlight ipython %}
In [1]: d1 = {'name': 'andy'}

In [2]: d2 = {'name': 'andy'}

In [3]: l = [d1, d2]

In [4]: print l
[{'name': 'andy'}, {'name': 'andy'}]

# way 1 - with set/tuple:
In [5]: l1 = [dict(t) for t in set([tuple(d.items()) for d in l])]

In [6]: print l1
[{'name': 'andy'}]

# way 2 - with enumerate:
In [7]: l2 = [i for n, i in enumerate(l) if i not in l[n+1:]]

In [8]: print l2
[{'name': 'andy'}]
{% endhighlight %}

See stackoverflow thread [Remove duplicate dict in list in Python](http://stackoverflow.com/questions/9427163/remove-duplicate-dict-in-list-in-python)
