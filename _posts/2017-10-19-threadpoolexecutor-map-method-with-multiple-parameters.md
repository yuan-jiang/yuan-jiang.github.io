---
layout: post
title: ThreadPoolExecutor map method with multiple parameters
date: 2017-10-19 09:23:00 +0800
tags: concurrent multithreading python
---

ThreadPoolExeuctor from concurrent.futures package in Python 3 is very useful for executing a task (function) with a set of data (parameter) concurrently and this post lists examples on how to pass MULTIPLE parameters to the task being executed.

## Pass by same length iterables
{% highlight python %}
from concurrent.futures import ThreadPoolExecutor
import threading

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

prefix = "test_"

def add_prefix(prefix, i):
    print("%s: %s%s" % (threading.get_ident(), prefix, i))


if __name__ == "__main__":
    with ThreadPoolExecutor(10) as pool:
        pool.map(add_prefix, [prefix] * len(data), data)

{% endhighlight %}

## Pass with wrapper lambda function
{% highlight python %}
from concurrent.futures import ThreadPoolExecutor
import threading

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

prefix = "test_"

def add_prefix(prefix, i):
    print("%s: %s%s" % (threading.get_ident(), prefix, i))


if __name__ == "__main__":
    with ThreadPoolExecutor(10) as pool:
        args = ((prefix, i) for i in data)
        pool.map(lambda p: add_prefix(*p), args)
{% endhighlight %}

## Pass with repeat function from itertools
{% highlight python %}
from concurrent.futures import ThreadPoolExecutor
import threading
from itertools import repeat

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

prefix = "test_"

def add_prefix(prefix, i):
    print("%s: %s%s" % (threading.get_ident(), prefix, i))


if __name__ == "__main__":
    with ThreadPoolExecutor(10) as pool:
        pool.map(add_prefix, repeat(prefix), data)
{% endhighlight %}

## References
- [concurrent.futures -- Launching parallel tasks](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.map)
- [Pass multiple parameters to concurrent.futures.Executor.map?](https://stackoverflow.com/questions/6785226/pass-multiple-parameters-to-concurrent-futures-executor-map)