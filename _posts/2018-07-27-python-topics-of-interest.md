---
layout: post
title: Python topics of interest
date: 2018-07-27 17:00:00 +0800
tags: python
---

List of python topics of interest.

## Understanding dis.dis
- [How should I understand the output of dis.dis?](https://stackoverflow.com/a/47529318)
- Why python tuple is faster than list by dis.dis:
{% highlight python %}
In [1]: f1 = lambda: (1, 2, 3, 4, 5)

In [2]: f2 = lambda: [1, 2, 3, 4, 5]

In [3]: import dis

In [4]: dis.dis(f1)
  1           0 LOAD_CONST               1 ((1, 2, 3, 4, 5))
              2 RETURN_VALUE

In [5]: dis.dis(f2)
  1           0 LOAD_CONST               1 (1)
              2 LOAD_CONST               2 (2)
              4 LOAD_CONST               3 (3)
              6 LOAD_CONST               4 (4)
              8 LOAD_CONST               5 (5)
             10 BUILD_LIST               5
             12 RETURN_VALUE
{% endhighlight %}
## PEP 572
- [PEP 572 -- Assignment Expressions)(https://www.python.org/dev/peps/pep-0572/)
- [PEP 572 and decision-making in Python](https://lwn.net/Articles/757713/)
- [Transfer of power](https://mail.python.org/pipermail/python-committers/2018-July/005664.html)

## Python interpreter
- [Python Innards](https://tech.blog.aknin.name/category/my-projects/pythons-innards/)
