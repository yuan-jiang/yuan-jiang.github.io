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
Example testing:
{% highlight java %}
public class InitTest {

    public static void main(String[] args)
    {
        A a = new A(100);
    }
}

class A
{
    public int num1 = -1;

    public static int num2 = -2;

    static
    {
        System.out.println("Static init...");
        System.out.println("Static member surName = " + num2);
    }

    // initializer block
    {
        System.out.println("Outside constructor, member variable name = " + num1);
    }

    public A (int num)
    {
        System.out.println("Inside constructor, member variable name = " + this.num1);
        this.num1 = num;
        System.out.println("Inside constructor, member variable name = " + this.num1);
    }
}

// output:
// Static init...
// Static member surName = -2
// Outside constructor, member variable name = -1
// Inside constructor, member variable name = -1
// Inside constructor, member variable name = 100
{% endhighlight %}

## The general order of initialization for classes with inheritance
1. Parent static items;
2. Child static items;
3. Parent member variables;
4. Parent constructor;
5. Child member variables;
6. Child constructor.
Example testing:
{% highlight java %}
public class InitTest {

    public static void main(String[] args)
    {
        C c = new C();
    }
}

class P
{
    static
    {
        System.out.println("Parent static...");
    }

    {
        System.out.println("Parent member variables...");
    }

    public P()
    {
        System.out.println("Parent constructor...");
    }
}

class C extends P
{
    static
    {
        System.out.println("Child static...");
    }

    {
        System.out.println("Child member variables...");
    }

    public C()
    {
        System.out.println("Child constructor...");
    }
}

// output:
// Parent static...
// Child static...
// Parent member variables...
// Parent constructor...
// Child member variables...
// Child constructor...
{% endhighlight %}

## Static initialization block
- A static initialization block is a normal block of code enclosed in braces, { }, and preceded by the static keyword, e.g. static { code }.
- Static instantiation block has the advantage of providing capabilities such as error handling, for looping, and such complex logic for class variables instantiation.
- A class can have any number of static initialization blocks, and they can appear anywhere in the class body. The runtime system guarantees that static initialization blocks are called in the order that they appear in the source code.
- Static initialization blocks are always run (once) at class load time. They are not run again for every later instantiation of the class.
- The blocks are run in the order they appear in the source code mixed in with regular static variable statements.
- Note that you have block scope in a static initialization block, so there can be variables initialized resembling identical-looking ones outside the block.
- There is an alternative to static blocks — you can write a private static method:
  {% highlight java %}
  class Whatever
  {
    public static varType myVar = initializeClassVariable();

    private static varType initializeClassVariable()
    {

        // initialization code goes here
    }
  }
  {% endhighlight %}

## Instance members initialization
- Provide an initial value for a field in its declaration. This is the most normal way but has limitations that you cannot have complex logic such as error handling, looping and etc.
- Alternative 1: Instance variables can be initialized in constructors, where error handling or other logic can be used.
- Alternative 2: Initializer block:
  + Initializer blocks for instance variables look just like static initializer blocks, but without the static keyword: { ... }.
  + The Java compiler copies initializer blocks into every constructor. Therefore, this approach can be used to share a block of code between multiple constructors.
- Alternative 3: Final method:
  + A final method cannot be overridden in a subclass.
  + This is especially useful if subclasses might want to reuse the initialization method
  {% highlight java %}
  class Whatever
  {
    private varType myVar = initializeInstanceVariable();

    protected final varType initializeInstanceVariable()
    {

        // initialization code goes here
    }
  }
  {% endhighlight %}

## References
- [static initialization](http://1001javatips.com/staticinitialization.htm)
- [initialization order](http://1001javatips.com/initializationorder.htm)
- [Initialization of Classes and Interfaces](https://docs.oracle.com/javase/specs/jls/se7/html/jls-12.html#jls-12.4)
- [Initializing Fields](https://docs.oracle.com/javase/tutorial/java/javaOO/initial.html)
