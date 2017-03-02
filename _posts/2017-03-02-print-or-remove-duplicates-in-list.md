---
layout: post
title: Print or remove duplicates in list
date: 2017-03-02 13:43:00 +0800
author: Yuan Jiang
tags: python
---

How to quickly print out or remove duplicated items from given list in python.

## Remove duplicates only
{% highlight python %}
# simply change list to set, e.g.
list_with_dups = [1, 2, 3, 1]
list_without_dups = list(set(list_with_dups))
{% endhighlight %}

## Find out duplicates only
{% highlight python %}
import collections
list_with_dups = [1, 2, 3, 1]
print [item for item, count in collections.Counter(list_with_dups).items() if count > 1]
{% endhighlight %}

## Find out and remove duplicates
{% highlight python %}
list_with_dups = [1, 2, 3, 1]
dups = []
uniq = []
for item in list_with_dups:
    if item not in uniq:
        uniq.append(item)
    else:
        dups.append(item)
# print dups
print dups
# print unique list with dups removed
print uniq
{% endhighlight %}

Also see this stackoverflow [post](http://stackoverflow.com/a/9835819) for reference.
