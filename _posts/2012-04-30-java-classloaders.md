---
layout: post
title: Java classloaders
date: 2012-04-30 15:16:00 +0800
author: Yuan Jiang
tags: java
---

Java class loading and three classloaders.

## Java class loading in order
- 1.Bootstrap classloader
  - parent of all other classloaders (root)
  - runtime classes: rt.jar
  - internationalization classes i18n.jar
  - and others. (jre/lib)
- 2.Extension classloader
  - child of bootstrap classloader
  - extension classes (lib/ext or java.ext.dirs)
- 3.System/Application classloader
  - child of extension classloader
  - user classes (java.class.path system property):
    + default value, current directory (.)
    + CLASSPATH environment variable, overrides default value
    + the -classpath or -cp command line option, overrides CLASSPATH
    + jars specified by the -jar option, overrides all above values

## Java class loading mechanism
{% highlight text %}
"The Java platform uses a delegation model for loading classes. The basic idea is
that every class loader has a "parent" class loader. When loading a class, a
class loader first "delegates" the search for the class to its parent class
loader before attempting to find the class itself." -- from ref#3

"If the parent classloader cannot find a class or resource, only then does the
classloader attempt to find them locally. In effect, a classloader is responsible
for loading only the classes not available to the parent; classes loaded by a
classloader higher cannot refer to classes available lower." -- from ref#1

"In the Java 2 Platform, Standard Edition, v1.2 and later releases, class loaders
have a hierarchical relationship. Each class loader has a parent class loader.
When a class loader is asked to load a class or resource, it consults its parent
class loader before attempting to load the item itself. The parent in turn consults
its parent, and so on. So it is only after all of the ancestor class loaders cannot
find the item that the current class loader gets involved." -- from ref#4
{% endhighlight %}

## References
- [Do You Really Get Classloaders?](http://zeroturnaround.com/rebellabs/rebel-labs-tutorial-do-you-really-get-classloaders/2/)
- [How Classes are Found](https://docs.oracle.com/javase/8/docs/technotes/tools/findingclasses.html)
- [Understanding Extension Class Loading](https://docs.oracle.com/javase/tutorial/ext/basics/load.html)
- [Class Loading](http://docs.oracle.com/javase/jndi/tutorial/beyond/misc/classloader.html)
