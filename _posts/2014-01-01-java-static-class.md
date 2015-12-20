---
layout: post
title: Java static class
date: 2014-01-01 08:00:00 +0800
author: Yuan Jiang
tags: java
---

In Java, static fields and static methods are common but how about static class? Here is an example.

{% highlight java %}
public class Outer
{

    public static void main(String[] args)
    {
        // inner class must be instantiated with outer class instance
        Outer o = new Outer();
        Outer.Inner oi = o.new Inner();
        oi.printHelloWorld();

        // inner static class can be instantiated directly
        Outer.InnerStatic ois = new Outer.InnerStatic();
        ois.printHelloWorld();
        Outer.InnerStatic.printOK();
    }

    public class Inner
    {
        public void printHelloWorld()
        {
            System.out.println("Hello world from the inner class");
        }

        // inner classes cannot have static declarations so below is not allowed
        // public static void printOK()
        // {
        //     System.out.println("OK");
        // }
    }

    // static class can only be defined inside another class
    public static class InnerStatic
    {
        public void printHelloWorld()
        {
            System.out.println("Hello world from the inner static class");
        }

        public static void printOK()
        {
            System.out.println("OK");
        }
    }
}
{% endhighlight %}
