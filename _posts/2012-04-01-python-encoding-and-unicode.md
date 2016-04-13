---
layout: post
title: Python encoding and unicode
date: 2012-04-01 16:54:00 +0800
author: Yuan Jiang
tags: python
---

Python source code encoding and unicode in 2.x.

## Define python encoding
{% highlight text %}
Python will default to ASCII as standard encoding if no other
encoding hints are given.

To define a source code encoding, a magic comment must
be placed into the source files either as first or second
line in the file, such as:

      # coding=<encoding name>

or (using formats recognized by popular editors)

      #!/usr/bin/python
      # -*- coding: <encoding name> -*-

or

      #!/usr/bin/python
      # vim: set fileencoding=<encoding name> :

More precisely, the first or second line must match the regular
expression "^[ \t\v]*#.*?coding[:=][ \t]*([-_.a-zA-Z0-9]+)".
{% endhighlight %}

## Python 2.x unicode
- Unicode type:
  + basestring <= str or unicode
  + isinstance(obj, basestring) => isinstance(obj, (str, unicode))
  + unicode(string[, encoding, errors])
- Unicode literal:
  + u'àôî'
  + U'ÀÔÎ'
  + \x
  + \u

## References
- [PEP 263 -- Defining Python Source Code Encodings](https://www.python.org/dev/peps/pep-0263/)
- [Python 2.x Unicode Support](https://docs.python.org/2/howto/unicode.html#python-2-x-s-unicode-support)
- [Python Built-in Functions - basestring](https://docs.python.org/2/library/functions.html#basestring)
