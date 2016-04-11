---
layout: post
title: Java parse common data files
date: 2013-03-07 17:10:00 +0800
author: Yuan Jiang
tags: java
---

Java parse common data files such csv, json, yaml, xml, ini, properties and etc.

## Java read csv with [javacsv.jar](http://www.csvreader.com/java_csv.php)
{% highlight java %}
try
{
    CsvReader cr = new CsvReader("tmp/test.csv");

    // 1. read csv headers - assuming first row is header record
    if (cr.readHeaders())
    {
        String[] headers = cr.getHeaders();
        for (String header: headers)
        {
            System.out.print(header + " ");
        }
        System.out.println();
    }

    System.out.println("***Done reading headers***");

    // 2. read csv records
    while (cr.readRecord())
    {
        String[] record = cr.getValues();
        for (String column: record)
        {
            System.out.print(column + " ");
        }
        System.out.println();
    }

    System.out.println("***Done reading records***");

    cr.close();
}
catch (Exception e)
{
    e.printStackTrace();
}
{% endhighlight %}
