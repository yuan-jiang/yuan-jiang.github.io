---
layout: post
title: Python timestamp format
date: 2012-04-13 17:10:00 +0800
author: Yuan Jiang
tags: python
---

How to display timestamp in desired format in python.

Example using the 'time' module:
{% highlight ipython %}
In [1]: import time

In [2]: date = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())

In [3]: print date
2012-04-13 17:07:16
{% endhighlight %}

Example using the 'datetime' module:
{% highlight ipython %}
In [1]: from datetime import datetime

In [2]: now = datetime.now()

In [3]: now.strftime("%Y-%m-%d %H:%M:%S")
Out[3]: '2012-04-13 17:29:34'
{% endhighlight %}

List of format directives (time module):
{% highlight text %}
%a => Locale's abbreviated weekday name, e.g. Thu
%A => Locale's full weekday name, e.g. Thursday
%b => Locale's abbreviated month name, e.g. Apr
%B => Locale's full month name, e.g. April
%c => Locale's appropriate date and time representation, e.g. Mon Apr 13 17:14:07 2012
%x => Locale's appropriate date representation, e.g. 04/13/12
%X => Locale's appropriate time representation, e.g. 17:20:31
%d => Day of the month as a decimal number [01,31]
%H => Hour (24-hour clock) as a decimal number [00,23]
%I => Hour (12-hour clock) as a decimal number [01,12]
%j => Day of the year as a decimal number [001,366]
%m => Month as a decimal number [01,12]
%M => Minute as a decimal number [00,59]
%p => Locale's equivalent of either AM or PM
%S => Second as a decimal number [00,61]
%U => Week number of the year as a decimal number [00,53] (Sunday as first day of week)
%w => Weekday as a decimal number [0(Sunday),6]
%W => Week number of the year as a decimal number [00,53] (Monday as first day of week)
%y => Year without century as a decimal number [00,99]
%Y => Year with century as a decimal number
%Z => Time zone name
%% => Literal % character
{% endhighlight %}

See python library reference for ['time' module](https://docs.python.org/2/library/time.html) and ['datetime' module](https://docs.python.org/2/library/datetime.html) for more details.
