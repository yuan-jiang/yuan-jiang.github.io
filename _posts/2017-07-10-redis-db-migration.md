---
layout: post
title: Redis db migration
date: 2017-07-10 14:05:00 +0800
tags: redis
---

A list of commands to do a quick migration for redis.

## redis instance - source side
{% highlight bash %}
# persist current db data to disk
$ redis-cli bgsave
# stop redis server
$ /etc/init.d/redis-server stop
# copy/scp the dump.rdb and dump.rdb_save to a location
$ copy/scp /var/lib/redis/dump.* <new-location>
# [optional] also copy redis custom config if any
$ copy/scp /etc/redis/redis.config <new-location>
{% endhighlight %}

## redis instance - destination side
{% highlight bash %}
# stop redis server !!! important
$ redis-server stop
# copy/scp the dump.* files to /var/lib/redis
$ copy/scp dump.* /var/lib/redis
# copy/scp the redis.config to /etc/redis/
$ copy/scp redis.config /etc/redis/
# start redis-server
$ redis-server start
# check db info to ensure migration is successful
$ redis-cli
$ 127.0.0.1:6379> INFO
{% endhighlight %}

See also an article [redis backup and restore](http://zdk.blinkenshell.org/redis-backup-and-restore/)