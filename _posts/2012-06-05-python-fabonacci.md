---
layout: post
title: Python fabonacci
date: 2012-06-05 16:57:00 +0800
author: Yuan Jiang
tags: python algorithm
---

Python fabonacci implementations.

Regular with recursion
{% highlight python %}
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)
{% endhighlight %}

One-liner with recursion
{% highlight python %}
f = lambda n: n if n < 2 else f(n-1) + f(n-2)
{% endhighlight %}

Sequence generator
{% highlight python %}
def fib(n):
    a, b = 0, 1
    for _ in xrange(n):
        yield a
        a, b = b, a + b
{% endhighlight %}

Sequence printer
{% highlight python %}
def fib(n):
    a, b = 0, 1
    for _ in xrange(n):
        print a,
        a, b = b, a + b
{% endhighlight %}

## References
- [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number)
- [Fibonacci Sequence](https://www.mathsisfun.com/numbers/fibonacci-sequence.html)
- [Python Fibonacci Generator](http://stackoverflow.com/questions/3953749/python-fibonacci-generator)
