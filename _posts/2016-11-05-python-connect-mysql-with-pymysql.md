---
layout: post
title: Python connect mysql with pymysql
date: 2016-11-05 19:25:00 +0800
author: Yuan Jiang
tags: mysql db database python
---

# Installation
{% highlight bash %}
$ pip install pymysql
{% endhighlight %}

# Create db connection
{% highlight python %}
import pymysql.cursors

# Connect to the database
connection = pymysql.connect(host='dbhost',
                             user='dbuser',
                             password='dbpass',
                             db='dbname',
                             charset='utf8mb4',
                             cursorclass=pymysql.cursors.DictCursor)
{% endhighlight %}

# Execute insert or update or delete sql
{% highlight python %}
with connection.cursor() as cursor:
    # Create a new record
    sql = "INSERT INTO `users` (`email`, `password`) VALUES (%s, %s)"
    cursor.execute(sql, ('me@yuanjiang.space', 'you-never-know'))

# connection is not autocommit by default. So you must commit to save
# your changes.
connection.commit()
{% endhighlight %}

# Execute select sql
{% highlight python %}
with connection.cursor() as cursor:
    # Read a single record
    sql = "SELECT `id`, `password` FROM `users` WHERE `email`=%s"
    cursor.execute(sql, ('webmaster@python.org',))
    result = cursor.fetchone()
    print(result)
{% endhighlight %}

# Close db connection
{% highlight python %}
try:
  pass
  # db operation
finally:
  connection.close()
{% endhighlight %}

# Reference
- [PyMySQL's documentation](http://pymysql.readthedocs.io/en/latest/index.html)
- [PyMySQL Github](https://github.com/PyMySQL/PyMySQL)
