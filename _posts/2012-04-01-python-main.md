---
layout: post
title: Python main
date: 2012-04-01 11:05:00 +0800
author: Yuan Jiang
tags: python
---

What the ```if __name__ == '__main__'```does in python and practice of defining
python main functions.

## What is ```if __name__ == '__main__'``` for?
{% highlight text %}
A python file can either be reusable module or be standalone executable script.
When python interpreter reads a python file, it will execute all its executable
statements, no matter it is being executed as standalone script or imported as
reusable module. If we want some of the executable statements to run only when
the module is being run as standalone script (for example, some test functions),
we can move them into if __name__ == '__main__'. When a file is being executed
as standalone script, its __name__ is set to __main__ by python interpreter, so
python interpreter will execute the statements within if __name__ == '__main__'
structure, while when it being imported, its __name__ is the script/module name,
and python interpreter does not run statements within if __name__ == '__main__'.
{% endhighlight %}

## Defining python main functions
{% highlight python %}
import sys
import getopt

class Usage(Exception):
    def __init__(self, msg):
        self.msg = msg

def main(argv=None):
    if argv is None:
        argv = sys.argv
    try:
        try:
            opts, args = getopt.getopt(argv[1:], "h", ["help"])
        except getopt.error, msg:
             raise Usage(msg)
        # more code, unchanged
    except Usage, err:
        print >>sys.stderr, err.msg
        print >>sys.stderr, "for help use --help"
        return 2

if __name__ == "__main__":
    sys.exit(main())
{% endhighlight %}

## References
- [Python Tutorial: ```if __name__=='__main__':```](http://www.bogotobogo.com/python/python_if__name__equals__main__.php)
- [What is ```'if __name__=="__main__":'``` for?](http://effbot.org/pyfaq/tutor-what-is-if-name-main-for.htm)
- [What does ```'if __name__=="__main__":'``` do?](http://stackoverflow.com/questions/419163/what-does-if-name-main-do)
- [Python main() functions by Guido van Rossum](https://www.artima.com/weblogs/viewpost.jsp?thread=4829)
