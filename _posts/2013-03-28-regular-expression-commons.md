---
layout: post
title: Regular expression commons
date: 2013-03-28 17:07:00 +0800
author: Yuan Jiang
tags: re regular-expression
---

Common regular expression symbols, characters, and usage.

## Symbols and special chars
{% highlight python %}
re1|re2        => match re1 or re2
.              => match any char (except \n)
^              => match start of string, same as \A
$              => match end of string, same as \Z
*              => match 0 or more occurrences of preceding regex
+              => match 1 or more occurrences of preceding regex
?              => match 0 or 1 occurrence of preceding regex
{N}            => match N occurrences of preceding regex
{M,N}          => match from M to N occurrences of preceding regex
{N,}           => match N or more occurrences of preceding regex
[...]          => match any single char from class
[..x-y..]      => match any single char in the range from x to y
[^...]         => NOT match any char from class, including ranges
(...)          => match enclosed regex and save as subgroup
\d             => match any decimal digit, same as [0-9]
\D             => NOT match any decimal digit, same as [^0-9]
\w             => match any alphanumeric char, same as [A-Za-z0-9_]
\W             => NOT match any alphanumeric char
\s             => match any whitespace char
\S             => NOT match any whitespace char
\b             => match any word boundary (\B is inverse of \b)
\N             => match saved subgroup N
\c             => match any special char
{% endhighlight %}

## References
- [Regular Expression Cheatsheet](http://www.rexegg.com/regex-quickstart.html)
