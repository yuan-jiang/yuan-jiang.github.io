---
layout: post
title: Java serialization
date: 2012-08-19 14:30:00 +0800
author: Yuan Jiang
tags: java
---

Java serialization and deserialization examples.

{% highlight java %}
import java.io.*;

/**
 * @author yuanjiang
 */
public class SeriDemo
{
    public static void main(String[] args)
    {
        try
        {
            // serialization
            // Student s = new Student();
            // s.name = "andy";
            // s.number = 100;
            // s.score = 99.5;
            // s.id = 112233;
            // FileOutputStream fos = new FileOutputStream("res/out.ser");
            // ObjectOutputStream oos = new ObjectOutputStream(fos);
            // oos.writeObject(s);
            // oos.close();
            // fos.close();

            // deserialization
            FileInputStream fis = new FileInputStream("res/out.ser");
            ObjectInputStream ois = new ObjectInputStream(fis);
            Student s = (Student) ois.readObject();
            System.out.println(s.name);
            System.out.println(s.number);
            System.out.println(s.score);
            System.out.println(s.id);
            s.says();
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }
}

class Student implements Serializable
{
    public String name;
    public int number;
    public double score;
    public transient int id;

    public void says()
    {
        System.out.println("hello");
    }
}
{% endhighlight %}
