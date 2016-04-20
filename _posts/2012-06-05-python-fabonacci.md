---
layout: post
title: Python fabonacci
date: 2012-06-05 16:57:00 +0800
author: Yuan Jiang
tags: python algorithm
---

Python fabonacci implementations.

Regular
{% highlight python %}
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)
{% endhighlight %}

One-liner
{% highlight python %}
f = lambda n: n if n < 2 else f(n-1) + f(n-2)
{% endhighlight %}

## References
- [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number)
- [Fibonacci Sequence](https://www.mathsisfun.com/numbers/fibonacci-sequence.html)
