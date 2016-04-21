---
layout: post
title: Java initialization
date: 2012-05-01 17:54:00 +0800
author: Yuan Jiang
tags: java
---

Java initialization.

## The general order of class initialization
1. First all static items are initialized, in their source code order;
2. Then all member variables are initialized;
3. Lastly the constructor is called.

## The general order of initialization for classes with inheritance
1. Parent static items;
2. Child static items;
3. Parent member variables;
4. Parent constructor;
5. Child member variables;
6. Child constructor.

## Static initialization
- Static initialization blocks consist of static { code }.
- Static initialization blocks are always run (once) at class load time. They are not run again for every later instantiation of the class.
- The blocks are run in the order they appear in the source code mixed in with regular static variable statements. See initialization order for a demonstration.
- Note that you have block scope in a static initialization block, so there can be variables initialized resembling identical-looking ones outside the block.

## References
- [static initialization](http://1001javatips.com/staticinitialization.htm)
- [initialization order](http://1001javatips.com/initializationorder.htm)
- [Initialization of Classes and Interfaces](https://docs.oracle.com/javase/specs/jls/se7/html/jls-12.html#jls-12.4)
- [Initializing Fields](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.html)
