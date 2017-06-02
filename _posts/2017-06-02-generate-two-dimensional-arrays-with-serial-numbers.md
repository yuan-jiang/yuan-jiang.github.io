---
layout: post
title: Generate two dimensional arrays with serial numbers
date: 2017-06-02 17:08:00 +0800
tags: python algorithm
---

Generate n*n two dimensional arrays with serial numbers in python.

{% highlight python %}
#!/usr/bin/env python3 -B


def gen_2d_array(n):
    d = [[] for _ in range(n)]
    for i in range(0, n * n):
        r = i // n
        d[r].append(i + 1)
    return d

print(gen_2d_array(abs(int(input("Enter n: ")))))

{% endhighlight %}

{% highlight bash %}
$ ./test_2darrays.py
Enter n: 10
[[1, 2, 3, 4, 5, 6, 7, 8, 9, 10], [11, 12, 13, 14, 15, 16, 17, 18, 19, 20], [21, 22, 23, 24, 25, 26, 27, 28, 29, 30], [31, 32, 33, 34, 35, 36, 37, 38, 39, 40], [41, 42, 43, 44, 45, 46, 47, 48, 49, 50], [51, 52, 53, 54, 55, 56, 57, 58, 59, 60], [61, 62, 63, 64, 65, 66, 67, 68, 69, 70], [71, 72, 73, 74, 75, 76, 77, 78, 79, 80], [81, 82, 83, 84, 85, 86, 87, 88, 89, 90], [91, 92, 93, 94, 95, 96, 97, 98, 99, 100]]
$
{% endhighlight %}