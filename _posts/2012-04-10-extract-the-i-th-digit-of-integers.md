---
layout: post
title: Extract the i-th digit of integers
date: 2012-04-10 16:32:00 +0800
author: Yuan Jiang
tags: algorithm python java
---

Extract the i-th digit of any integers, where i starts from right to left.

Python
{% highlight python %}
In [1]: y = lambda x, i: (x/10**(i-1))%10

In [2]: y(8765,3)
Out[2]: 7
{% endhighlight %}


Java
{% highlight java %}
/**
 * Get the i-th (from right to left) digit of number
 * @param number the integer number
 * @param i the position, should be greater than 0
 * @return the i-th digit of given integer number
 */
public static int getIthDigit(int number, int i)
{
    return (number/(int)Math.pow(10, i-1))%10;
}
{% endhighlight %}
