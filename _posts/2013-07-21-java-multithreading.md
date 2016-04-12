---
layout: post
title: Java multithreading
date: 2013-07-21 11:29:00 +0800
author: Yuan Jiang
tags: java multithreading
---

Java multithreading programming demo.

{% highlight java %}

/**
 * Created by yuanjiang on 7/21/13.
 */
public class ThreadDemo
{
    public static void main(String[] args)
    {
        Thread t_A = new Thread(new A());
        t_A.start();

        Thread t_B = new Thread(new B());
        t_B.start();

        for (int i=0; i<10; i++)
        {
            System.out.println("main is printing...");
            try
            {
                Thread.sleep(500);
            }
            catch (InterruptedException e)
            {
                e.printStackTrace();
            }
        }
    }
}



// 1 - extending Thread class
class A extends Thread
{
    @Override
    public void run()
    {
        for (int i=0; i<10; i++)
        {
            System.out.println("A is printing...");
            try
            {
                Thread.sleep(400);
            }
            catch (InterruptedException e)
            {
                e.printStackTrace();
            }
        }
    }
}


// 2 - implementing Runnable interface
class B implements Runnable
{

    @Override
    public void run() {
        for (int i=0; i<10; i++)
        {
            System.out.println("B is printing...");
            try
            {
                Thread.sleep(300);
            }
            catch (InterruptedException e)
            {
                e.printStackTrace();
            }

        }
    }
}
{% endhighlight %}
