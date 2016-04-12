---
layout: post
title: Java parse common data files
date: 2013-03-07 17:10:00 +0800
author: Yuan Jiang
tags: java
---

Java parse common data files such as csv, json, yaml, xml, ini, properties and etc.

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

## Java read json with [gson](https://github.com/google/gson)
- Json file example:
{% highlight json %}
{
    "host": "www.google.com",
    "creds": [
        {
            "user": "admin",
            "pass": "admin123"
        },
        {
            "user": "test",
            "pass": "test123"
        }
        ]
}
{% endhighlight %}

- Java code to read all member values:
{% highlight java %}
try
{
    JsonParser jp = new JsonParser();
    JsonElement je = jp.parse(new FileReader("tmp/test.json"));
    JsonObject jo = je.getAsJsonObject();

    // 1. get value
    System.out.println(jo.get("host"));

    // 2. get list of values
    JsonArray ja = jo.get("creds").getAsJsonArray();
    Iterator<JsonElement> jes = ja.iterator();
    while (jes.hasNext())
    {
        JsonElement jsonElement = jes.next();
        JsonObject jsonObject = jsonElement.getAsJsonObject();
        System.out.println(jsonObject.get("user"));
        System.out.println(jsonObject.get("pass"));
    }
}
catch (Exception e)
{
    e.printStackTrace();
}
{% endhighlight %}

## Java read yaml with [snakeyaml](https://bitbucket.org/asomov/snakeyaml)
- Yaml file example:
{% highlight yaml %}
host: www.google.com
creds:
  - user: admin
    pass: admin123
  - user: test
    pass: test123
{% endhighlight %}

- Java code to read all member values:
{% highlight java %}
try
{
    Yaml yaml = new Yaml();

    Map<String, Object> data = (Map<String, Object>)yaml.load(new FileReader("tmp/test.yml"));

    // 1. get value
    System.out.println(data.get("host"));

    // 2. get list of values
    List<Map> creds = (List<Map>)data.get("creds");
    for (Map cred: creds)
    {
        System.out.println(cred.get("user"));
        System.out.println(cred.get("pass"));
    }

}
catch (Exception e)
{
    e.printStackTrace();
}
{% endhighlight %}
