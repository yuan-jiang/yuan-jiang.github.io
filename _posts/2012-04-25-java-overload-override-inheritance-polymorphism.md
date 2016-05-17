---
layout: post
title: Java overload override inheritance polymorphism
date: 2012-04-25 13:53:00 +0800
author: Yuan Jiang
tags: java
---

Java overload, override, inheritance, and polymorphism.

## overload
- Multiple methods with the same name but different in:
  + parameters number;
  + or
  + parameters type;
- Excluding the following cases:
  + different parameters name;
  + different return type;

## override
- Subclass declares methods that are already present in its parent class:
  + with argument list (type and sequence) the same as that of the parent;
  + with return type the same or subtype of that of the parent;
  + with narrower or fewer checked exceptions than that of the parent;
  + and access modifier not being more restrictive than that of the parent;
- Except for the following cases:
  + method declared 'final' cannot be overridden;
  + method declared 'static' cannot be overridden;
  + method declared 'private' cannot be inherited therefore not overridden;
  + constructors cannot be overridden;

## inheritance
- The extends keyword is used for inheritance;
- Child class derives from parent class with all nonstatic/nonprivate members;

## polymorphism
- Child inherits and overrides parent method;
- Parent variable references child and the behavior is based on child method;

## example
{% highlight java %}
class Triangle extends Shape
{
      // method overriding
      public int getSides()
      {
        return 3;
      }
}

class Rectangle extends Shape
{
      public int getSides(int i)
      {
        return i;
      }
}

public class Shape
{
      public boolean isShape()
      {
        return true;
      }

      public int getSides()
      {
        return 0;
      }

      // method overloading      
      public int getSides(Triangle tri)
      {
        return 3;
      }

      public int getSides(Rectangle rec)
      {
        return 4;
      }

      public static void main(String[] args)
      {
            Triangle triangle = new Triangle();
            System.out.println(triangle.isShape()); // inheritance

            Shape shape = new Triangle();
            System.out.println(shape.getSides());  // polymorphism
      }
}
{% endhighlight %}
