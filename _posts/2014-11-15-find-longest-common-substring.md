---
layout: post
title: Find longest common substring
date: 2014-11-15 14:47:00 +0800
author: Yuan Jiang
tags: algorithm python
---

Find max common substring(s).

Python example:
{% highlight python %}
#!/usr/bin/env python


def find_common_substrings(str1, str2):
    common_substrings = []
    long_str, short_str = (str1, str2) if len(str1) > len(str2) else (str2, str1)
    for i in range(len(short_str)):
        tmp_substring = short_str[i:]
        substrings_of_1stchar = [tmp_substring[:i+1] for i in range(len(tmp_substring))]
        for substring in substrings_of_1stchar:
            if substring in long_str:
                common_substrings.append(substring)
    return common_substrings


if __name__ == '__main__':
    str1 = raw_input("Enter str1: ")
    str2 = raw_input("Enter str2: ")
    if len(str1) < 1 or len(str2) < 1:
        print 'Empty string received!'
    else:
        subs = find_common_substrings(str1, str2)
        if subs:
            subs.sort(cmp=None, key=len, reverse=True)
            print 'Max common sub string(s):'
            # after sort in descending order len of 1st substring is the longest
            max_sub_len = len(subs[0])
            for sub in subs:
                if len(sub) == max_sub_len:
                    print sub
                else:
                    break
        else:
            print 'No common sub string(s)!'


##test and output
# Enter str1: hellopythonworld
# Enter str2: intheworldpeoplesayhello
# Max common sub string(s):
# hello
# world
{% endhighlight %}

See also wikipedia [Longest common substring problem](https://en.wikipedia.org/wiki/Longest_common_substring_problem)
