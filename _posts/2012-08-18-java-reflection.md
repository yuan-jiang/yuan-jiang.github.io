---
layout: post
title: Java reflection
date: 2012-08-18 10:00:00 +0800
author: Yuan Jiang
tags: java
---

Java reflection examples.

## Accessing java private fields and methods with Class and reflect
{% highlight java %}
import java.lang.reflect.Field;
import java.lang.reflect.Method;

/**
 * Created by yuanjiang on 8/18/12.
 */
public class ClassDemo
{
    public static void main(String[] args) throws Exception
    {
        // A a = new A();
        // Class b = a.getClass();  // 1 - get class

        // Class b = A.class;  // 2 - get class

        Class b = Class.forName("A"); // 3 - get class

        Method m = b.getDeclaredMethod("say", String.class);
        m.setAccessible(true);

        // m.invoke(a, "hello");
        m.invoke(b.newInstance(), "hello");

        Field f = b.getDeclaredField("i");
        f.setAccessible(true);
        // f.setInt(a, 100);
        A c = (A)b.newInstance();
        f.setInt(c, 1000);
        System.out.println(f.get(c));
    }
}


class A
{
    private int i=9;
    private void say(String words)
    {
        System.out.println("...say..." + words);
    }
}
{% endhighlight %}
