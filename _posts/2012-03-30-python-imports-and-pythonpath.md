---
layout: post
title: Python imports and PYTHONPATH
date: 2012-03-30 13:28:00 +0800
author: Yuan Jiang
tags: python
---

Python imports and PYTHONPATH.

## Python imports
- "import foo.bar.x" style:
  + Full name of "foo.bar.x" must be used to refer to what's imported;
  + Each item except for the last one ("x") must be a package;
  + The "x" can be a module or a package;
  + The "x" can't be any of class, function, or variable defined in 2nd last item.
{% highlight ipython %}
In [1]: import selenium.webdriver.firefox

In [2]: selenium.webdriver.firefox
Out[2]: <module 'selenium.webdriver.firefox' from '/Library/Python/2.7/site-packages/selenium/webdriver/firefox/__init__.pyc'>

In [3]: firefox
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-3-38144139eb6a> in <module>()
----> 1 firefox

NameError: name 'firefox' is not defined
{% endhighlight %}

- "from foo.bar import x" style
  + "x" is now only directly available for use;
  + "x" can be either a module or a subpackage;
  + "x" can also be function, class, variable defined in the module.
{% highlight ipython %}
In [1]: from selenium.webdriver import firefox

In [2]: firefox
Out[2]: <module 'selenium.webdriver.firefox' from '/Library/Python/2.7/site-packages/selenium/webdriver/firefox/__init__.pyc'>

In [3]: selenium.webdriver.firefox
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-3-f5e8eabbe74b> in <module>()
----> 1 selenium.webdriver.firefox

NameError: name 'selenium' is not defined
{% endhighlight %}


- Package-relative import with . and .. using "from foo.bar import x" style:
{% highlight python %}
# Assuming code in A.B.C module is doing the following imports:
from . import D     # imports A.B.D
from .. import E    # imports A.E
from ..F import G   # imports A.F.G
{% endhighlight %}

## Python PYTHONPATH
1. Description:
- Environment variable like linux PATH;
- Defines search path for python interpreter to look for modules to import;
- NOTE that the following are always available in search path:
  + Python installation site-packages directory;
  + Current directory;
  + Current module's directory (relative import to be possible).
2. Set PYTHONPATH:
{% highlight bash %}
export PYTHONPATH=$PYTHONPATH:/home/user/testdir
{% endhighlight %}


## References
- [PEP 328: Absolute and Relative Imports](https://docs.python.org/2.5/whatsnew/pep-328.html)
- [Python Modules](https://docs.python.org/2/tutorial/modules.html)
