---
layout: post
title: SQL case statement
date: 2017-05-09 21:13:00 +0800
tags: sql
---

How to use CASE statement in sql select statement to deal with if-then-else logic.

{% highlight sql %}
INSERT INTO table_new(newColumnA, newColumnB, newColumnC, newColumnD)
SELECT columnA AS newColumnA,
       columnB AS newColumnB,
       CASE columnC
           WHEN 'value1' THEN 'newValue1'
           WHEN 'value2' THEN 'newValue2'
           ELSE 'newDefaultValue'
       END AS newColumnC,
       columnD AS newColumnD
FROM table_old
{% endhighlight %}

See [reference](http://stackoverflow.com/questions/5487892/sql-server-case-when-or-then-else-end-the-or-is-not-supported) for more.