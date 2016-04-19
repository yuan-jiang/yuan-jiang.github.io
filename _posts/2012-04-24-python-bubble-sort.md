---
layout: post
title: Python bubble sort
date: 2012-04-24 10:10:00 +0800
author: Yuan Jiang
tags: python algorithm
---

Bubble sort algorithm is comparison based algorithm in which each pair of adjacent elements is compared and elements are swapped if they are not in order. The average and worst case complexity of bubble sort is O(n^2) where n is the number of items.

## Python bubble sort examples

- Example 1 - basic
{% highlight ipython %}
# ascending order:
In [1]: x = [7,2,8,9,4,3,5,6,1]

In [2]: print x
[7, 2, 8, 9, 4, 3, 5, 6, 1]

In [3]: for i in range(len(x)):
   ...:     for j in range(len(x)-1-i):
   ...:         if x[j] > x[j+1]:
   ...:             x[j], x[j+1] = x[j+1], x[j]
   ...:             

In [4]: print x
[1, 2, 3, 4, 5, 6, 7, 8, 9]

# descending order:
In [5]: x = [7,2,8,9,4,3,5,6,1]

In [6]: print x
[7, 2, 8, 9, 4, 3, 5, 6, 1]

In [7]: for i in range(len(x)):
   ...:     for j in range(len(x)-1-i):
   ...:         if x[j] < x[j+1]:
   ...:             x[j], x[j+1] = x[j+1], x[j]
   ...:             

In [8]: print x
[9, 8, 7, 6, 5, 4, 3, 2, 1]
{% endhighlight %}

- Example 2 - improved
{% highlight ipython %}
In [1]: x = [6,1,2,3,4,5]

In [2]: for i in range(len(x)):
   ...:     sorted = True
   ...:     for j in range(len(x)-1-i):
   ...:         if x[j] > x[j+1]:
   ...:             x[j], x[j+1] = x[j+1], x[j]
   ...:             sorted = False
   ...:     if sorted:
   ...:         break
   ...:     

In [3]: print x
[1, 2, 3, 4, 5, 6]
{% endhighlight %}

## References
- [Data Structure - Bubble Sort Algorithm](http://www.tutorialspoint.com/data_structures_algorithms/bubble_sort_algorithm.htm)
- [Bubble Sort Program in C](http://www.tutorialspoint.com/data_structures_algorithms/bubble_sort_program_in_c.htm)
- [BUBBLE SORT in Java and C++](http://www.algolist.net/Algorithms/Sorting/Bubble_sort)
