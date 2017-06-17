---
layout: post
title: Huey as crontab alternative in python
date: 2017-06-17 17:05:00 +0800
tags: python
---

Huey is a little task queue that supports:  
- multi-process, multi-thread or greenlet task execution models
- schedule tasks to execute at a given time, or after a given delay
- schedule recurring tasks, like a crontab
- automatically retry tasks that fail
- task result storage
- consumer publishes event stream, allowing high-fidelity monitoring

How to use:

## Installation
{% highlight bash %}
# install redis acts as queue for task storage in huey
$ sudo apt-get install redis-server
$ sudo /etc/init.d/redis-server start

# install huey
$ pip install huey

# [optional] install greenlet if for IO-bound tasks
$ pip install greenlet
{% endhighlight %}

## Create huey config
{% highlight python %}
# config.py
from huey import RedisHuey

huey = RedisHuey('test', host='localhost')

{% endhighlight %}

## Create tasks
{% highlight python %}
# tasks.py
from config import huey
from huey import crontab
from datetime import datetime

@huey.periodic_task(crontab(minute="*/1"))
def print_hello():
    print("hello", datetime.now())


@huey.periodic_task(crontab(minute="*/1"))
def print_world():
    print("world", datetime.now())

@huey.periodic_task(crontab(minute="*/1"))
def print_ok():
    print("ok", datetime.now())

{% endhighlight %}

## Create main executor
{% highlight python %}
from config import huey
from tasks import print_hello, print_world, print_ok


if __name__ == "__main__":
    print_hello()
    print_world()
    print_ok()
{% endhighlight %}

## Run scheduler
{% highlight bash %}
# with process for cpu-intensive tasks
$ huey_consumer.py main.huey -k process -w 4

# with gevent for io-bound tasks
$ huey_consumer.py main.huey -k greenlet -w 10

# save log to file
$ huey_consumer.py main.huey -k process -w 4 --logfile=../logs/huey.log
{% endhighlight %}

## References
- [Huey](https://huey.readthedocs.io/en/latest/index.html)
- [Huey’s API](https://huey.readthedocs.io/en/latest/api.html)
- [Manage by supervisord](https://github.com/coleifer/huey/issues/88)


