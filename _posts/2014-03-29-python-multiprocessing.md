---
layout: post
title: Python multiprocessing
date: 2014-03-29 17:19:00 +0800
author: Yuan Jiang
tags: multiprocessing python
---

Python multiprocessing.

## Python multiprocessing example with ThreadPool
{% highlight python %}
class TestUtil:

    def say_hello(self):
        return 'hello'

    def say_number(self):
        return 1

    def do_sum(self):
        return 100 + 200

    def do_subtract(self):
        return 100000 - 2323230

    def do_multiply(self):
        return 232030 * 20

    def do_divide(self):
        return 32320 / 2320

if __name__ == "__main__":

    import time

    from multiprocessing.pool import ThreadPool
    pool = ThreadPool(processes=1)

    start = time.time()

    for m in TestUtil.__dict__.values():
        try:
            async_result = pool.apply_async(m, (None,))
            print async_result.get()
        except TypeError:
            pass

    end = time.time()

    print 'time spent: ', end - start
{% endhighlight %}
