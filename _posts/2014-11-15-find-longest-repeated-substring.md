---
layout: post
title: Find longest repeated substring
date: 2014-11-15 16:21:00 +0800
author: Yuan Jiang
tags: algorithm python
---

Find max/longest substring for a given string with minimum occurrences.

Python example (functional but not elegant)
{% highlight python %}
#!/usr/bin/env python

def find_longest_substrings(parent, min_occurrences=2):
    targets = []
    substrings = []
    for i in range(len(parent)):
        tmp_substring = parent[i:]
        substrings_of_1stchar = [tmp_substring[:i+1] for i in range(len(tmp_substring))]
        substrings += substrings_of_1stchar

    substrings_in_occurrences = []
    for sub in substrings:
        if substrings.count(sub) >= min_occurrences:
            substrings_in_occurrences.append(sub)
    if substrings_in_occurrences:
        unique_substrings_in_occurrences = [item for item in set(substrings_in_occurrences)]
        unique_substrings_in_occurrences.sort(key=len, reverse=True) # sort in desc
        max_sub_len = len(unique_substrings_in_occurrences[0])
        for sub in unique_substrings_in_occurrences:
            if len(sub) == max_sub_len:
                targets.append(sub)
            else:
                break
    return targets

if __name__ == '__main__':
    parent = raw_input("Enter str: ")
    occ = int(raw_input("Enter occ: "))
    print 'Longest substring(s) with occurrence(s) is:'
    print find_longest_substrings(parent, occ)

##test and output
# Enter str: imtestinghelloworldpythonandhelloworldshouldappearatleastthreetimeshelloworldfinally
# Enter occ: 3
# Longest substring(s) with occurrence(s) is:
# ['helloworld']

{% endhighlight %}

## References
- [Longest repeated substring problem](https://en.wikipedia.org/wiki/Longest_repeated_substring_problem)
- [Suffix Tree Application 3 - Longest Repeated Substring](http://www.geeksforgeeks.org/suffix-tree-application-3-longest-repeated-substring/)
- [Find longest repetitive sequence in a string](http://stackoverflow.com/questions/11090289/find-longest-repetitive-sequence-in-a-string)
