---
layout: post
title: Java parse common data files
date: 2013-03-07 17:10:00 +0800
author: Yuan Jiang
tags: java csv json yaml xml ini properties
---

Java parse common data files such as csv, json, yaml, xml, ini, properties and etc.

## Java read csv with [javacsv.jar](http://www.csvreader.com/java_csv.php)
- Csv file example:
{% highlight text %}
host,username,password
1.1.1.1,admin,admin123
1.1.1.2,root,root123
{% endhighlight %}

- Java code to read data from csv file:
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

## Java read xml with javax.xml.parsers.* or dom4j
- Xml file example:
{% highlight xml %}
<task>
    <step>
        <command>passwd root</command>
        <expect>password:</expect>
    </step>
    <step>
        <command>root123</command>
        <expect>password:</expect>
    </step>
    <step>
        <command>root123</command>
        <expect>successfully.</expect>
    </step>
</task>
{% endhighlight %}

- Java read the shell sequence using DOM parser:
{% highlight java %}
import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

try
{
    DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
    DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();

    Document document = documentBuilder.parse(new File("tmp/test.xml"));

    NodeList nodeList = document.getElementsByTagName("step");
    for (int i=0; i<nodeList.getLength(); i++)
    {
        Node node = nodeList.item(i);
        NodeList childNodeList = node.getChildNodes();
        for (int j=0; j<childNodeList.getLength(); j++)
        {
            Node leafNode = childNodeList.item(j);
            if (leafNode.getNodeName().equalsIgnoreCase("COMMAND"))
            {
                System.out.println("COMMAND: " + leafNode.getTextContent());
            }
            if (leafNode.getNodeName().equalsIgnoreCase("EXPECT"))
            {
                System.out.println("EXPECT: " + leafNode.getTextContent());
            }
        }
    }

}
catch (Exception e)
{
    e.printStackTrace();
}
{% endhighlight %}

- Java read the shell sequence using SAX parser:
{% highlight java %}
import org.xml.sax.Attributes;
import org.xml.sax.helpers.DefaultHandler;

import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

public static void main(String[] args)
{

    try
    {
        SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
        SAXParser saxParser = saxParserFactory.newSAXParser();

        saxParser.parse(new File("tmp/test.xml"), new MySaxHandler());

    }
    catch (Exception e)
    {
        e.printStackTrace();
    }
}

static class MySaxHandler extends DefaultHandler
{
    boolean isCommand = false;
    boolean isExpect = false;

    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes)
    {
        if (qName.equalsIgnoreCase("command"))
        {
            isCommand = true;
        }

        if (qName.equalsIgnoreCase("expect"))
        {
            isExpect = true;
        }
    }

    @Override
    public void characters(char[] ch, int start, int length)
    {
        if (isCommand)
        {
            System.out.println("COMMAND: " + new String(ch, start, length));
            isCommand = false;
        }

        if (isExpect)
        {
            System.out.println("EXPECT: " + new String(ch, start, length));
            isExpect = false;
        }
    }
}
{% endhighlight %}

## Java read ini file with [ini4j](http://ini4j.sourceforge.net/)
- Ini file example:
{% highlight ini %}
[web]
host=1.1.1.1
username=admin
password=admin123

[db]
host=1.1.1.2
username=root
password=root123
{% endhighlight %}

- Java code to read config from ini file:
{% highlight java %}
try
{
    Ini ini = new Ini(new File("tmp/test.ini"));

    System.out.println("Reading web config...");
    System.out.println(ini.get("web", "host"));
    System.out.println(ini.get("web", "username"));
    System.out.println(ini.get("web", "password"));

    System.out.println("Reading db config...");
    System.out.println(ini.get("db", "host"));
    System.out.println(ini.get("db", "username"));
    System.out.println(ini.get("db", "password"));
}
catch (Exception e)
{
    e.printStackTrace();
}
{% endhighlight %}

## Java read properties file with Properties:
- Properties file example:
{% highlight properties %}
web.host = 1.1.1.1
web.username = admin
web.password = admin123
db.host = 1.1.1.2
db.username = root
db.password = root123
{% endhighlight %}

- Java code to read config from properties file:
{% highlight java %}
try
{
    Properties properties = new Properties();
    properties.load(new FileReader("tmp/test.properties"));

    System.out.println("Reading web config...");
    System.out.println(properties.get("web.host"));
    System.out.println(properties.get("web.username"));
    System.out.println(properties.get("web.password"));

    System.out.println("Reading db config...");
    System.out.println(properties.get("db.host"));
    System.out.println(properties.get("db.username"));
    System.out.println(properties.get("db.password"));
}
catch (Exception e)
{
    e.printStackTrace();
}
{% endhighlight %}
