---
layout: post
title: SQL functions
date: 2016-01-02 14:35:00 +0800
author: Yuan Jiang
tags: sql
---

SQL functions.

## COUNT
{% highlight sql %}
=> count number of values for specific column:
SELECT COUNT(column_name) FROM table_name;

=> count table records:
SELECT COUNT(*) FROM table_name;

=> count distinct values:
SELECT COUNT(DISTINCT column_name) FROM table_name;
{% endhighlight %}

## MAX/MIN/SUM/AVG
{% highlight sql %}
SELECT MAX(column_name) FROM table_name;
SELECT MIN(column_name) FROM table_name;
SELECT SUM(column_name) FROM table_name;
SELECT AVG(column_name) FROM table_name;

e.g. Select product with price above average:
SELECT ProductName, Price FROM Products
WHERE Price>(SELECT AVG(Price) FROM Products);
{% endhighlight %}

## GROUP BY statement
{% highlight sql %}
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
{% endhighlight %}

## HAVING clause
{% highlight sql %}
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
{% endhighlight %}

## UCASE/LCASE
{% highlight sql %}
SELECT UCASE(column_name) FROM table_name;
SELECT UPPER(column_name) FROM table_name; # sqlserver
SELECT LCASE(column_name) FROM table_name;
SELECT LOWER(column_name) FROM table_name; # sqlserver
{% endhighlight %}

## FIRST/LAST
{% highlight sql %}
SELECT FIRST(column_name) FROM table_name; # only in ms access
SELECT LAST(column_name) FROM table_name; # only in ms access

=> workaround for other db systems with ASC/DESC:
1) SELECT TOP 1 ... => sqlserver
2) LIMIT 1 ...      => mysql
3) ROWNUM<=1 ...    => oracle
{% endhighlight %}

## MID/LEN/ROUND/NOW/FORMAT
{% highlight sql %}
=> extract chars from text field:
SELECT MID(column_name,start,length) AS some_name FROM table_name;
SELECT SUBSTRING(column_name,start,length) AS some_name FROM table_name; # sqlserver

=> return length of text field:
SELECT LEN(column_name) FROM table_name;

=> round numeric field:
SELECT ROUND(column_name,decimals) FROM table_name;

=> return current system date and time:
SELECT NOW() FROM table_name;

=> format field display:
SELECT FORMAT(column_name,format) FROM table_name;
{% endhighlight %}

## References
- [SQL functions](http://www.w3schools.com/sql/sql_functions.asp)
- [SQL quick references](http://www.w3schools.com/sql/sql_quickref.asp)
