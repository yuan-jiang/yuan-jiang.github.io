---
layout: post
title: Insert into mysql with python dict
date: 2016-12-03 14:47:00 +0800
author: Yuan Jiang
tags: mysql python database
---

How to use python dict as data source for mysql insert-into statement.

{% highlight python %}
placeholders = ', '.join(['%s'] * len(dict_data))
columns = ', '.join(dict_data.keys())
sql = "INSERT INTO %s ( %s ) VALUES ( %s )" % (table, columns, placeholders)
cursor.execute(sql, dict_data.values())
{% endhighlight %}

See also this stackoverflow for [reference](http://stackoverflow.com/a/14834646)
