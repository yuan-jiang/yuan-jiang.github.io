---
layout: post
title: How to reset mysql root password
date: 2016-10-30 10:37:00 +0800
author: Yuan Jiang
tags: mysql database db
---

How to reset mysql root password.

## Use "mysqladmin" command line tool
{% highlight bash %}
$ mysqladmin -u root -pcurrentpassword password 'newpassword'
{% endhighlight %}

## Login with root user and use update statement
{% highlight sql %}
UPDATE user SET password=PASSWORD('newpassword') WHERE user='root';
{% endhighlight %}

## Use mysql-workbench 
- First login to mysql db with mysql-workbench
- Go to "Management" tab
- Select "Users and Privileges"
- Select "root" user and set password at "login" tab
