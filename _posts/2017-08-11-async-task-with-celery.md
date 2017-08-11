---
layout: post
title: Async task with celery
date: 2017-08-11 15:09:00 +0800
tags: celery python
---

A quick guide on how to use celery to accomplish async task in application.

## How it works for celery-based async tasks
- Decorated function as celery task
- Broker (e.g. rabbitmq, redis, and etc.) to queue up the task
- Celery worker on the background to actually execute the task

## Installation
{% highlight bash %}
# install a broker
$ sudo apt-get install redis-server [or rabbitmq-server]
$ sudo /etc/init.d/redis-server start
or $ sudo /etc/init.d/rabbitmq-server start

# install celery
$ sudo pip install celery
$ sudo pip install redis [if using redis as broker]
{% endhighlight %}


## Define a task
{% highlight python %}
# celery_tasks.py
from celery import Celery

celery_app = Celery('celery_tasks', broker='redis://localhost:6379/0')

@celery_app.task
def sum_to_one_million():
    return sum(range(1000000))

{% endhighlight %}

## Start worker
{% highlight bash %}
$ cd ~/workspace/celery_project
$ celery -A celery_tasks worker --loglevel=info
{% endhighlight %}

## Invoke the task
- in interactive shell
{% highlight python %}
>> from celery_tasks import sum_to_one_million
>> print(sum_to_one_million())
{% endhighlight %}

- in a flask app
{% highlight python %}
from flask import Flask
from celery_tasks import sum_to_one_million

app = Flask(__name__)

@app.route("/calcSync")
def calc_sum_sync():
    return sum_to_one_million()

@app.route("/calcAsync")
def calc_sum_async():
    sum_to_one_million.delay()
    return "Calculation started in the background."


{% endhighlight %}

## References
- [First Steps with Celery](http://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html#first-steps)
- [Using Celery with Flask](https://blog.miguelgrinberg.com/post/using-celery-with-flask)
- [Celery Based Background Tasks](http://flask.pocoo.org/docs/0.12/patterns/celery/)
- [How Celery fixed Python's GIL problem](http://blog.domanski.me/how-celery-fixed-pythons-gil-problem/)


