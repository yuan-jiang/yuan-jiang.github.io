---
layout: post
title: MySQL delete table rows and reset index
date: 2018-01-24 11:15:00 +0800
tags: mysql db
---

How to delete all rows of a table and also reset the index.

## With TRUNCATE TABLE command
{% highlight SQL %}
TRUNCATE TABLE `table-name`;
{% endhighlight %}

## With DELETE FROM command
{% highlight SQL %}
-- FIRST DELETE ALL ROWS
DELETE FROM `table-name`;
-- THEN RESET INDEX TO START FROM 1 AGAIN
ALTER TABLE `table-name` AUTO_INCREMENT = 1;
{% endhighlight %}

See [here](https://stackoverflow.com/questions/12651867/mysql-delete-all-rows-from-table-and-reset-id-to-zero) also for reference.