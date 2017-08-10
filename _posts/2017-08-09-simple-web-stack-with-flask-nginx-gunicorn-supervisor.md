---
layout: post
title: Simple web stack with flask nginx gunicorn supervisor
date: 2017-08-09 16:36:00 +0800
tags: flask nginx gunicorn supervisor
---

A simple web stack using flask, nginx, gunicorn and supervisor in python with minimum configuration. This is useful if you want to QUICKLY setup a simple but production ready web service.

## Create web app
{% highlight python %}
# simpleapp.py
from flask import Flask, jsonify

app = Flask(__name__)

@app.route("/")
def index():
    return jsonify({
        "message": "Hello, World!"
    }), 200
{% endhighlight %}

## Install dependencies
{% highlight bash %}
# install system packages
$ sudo apt-get install nginx
$ sudo apt-get install gunicorn
$ sudo apt-get install supervisor
[optional] $ sudo apt-get install logrotate
# install python library
$ sudo pip install flask
{% endhighlight %}

## Configuration for nginx
{% highlight nginx %}
# simpleapp-nginx
# copy the config to: /etc/nginx/sites-enabled
# or create link: $ ln -s /path/to/simpleapp-nginx /etc/nginx/sites-enabled
server {
  listen 80;

  location / {
    include proxy_params;
    proxy_pass http://unix:/run/gunicorn/socket;
  }
}
{% endhighlight %}

## Configuration for supervisor
{% highlight ini %}
# simpleapp-supervisor.conf
# copy the config to: /etc/supervisor/conf.d
# or create link: $ ln -s /path/to/simpleapp-supervisor.conf /etc/supervisor/conf.d
[program:gunicorn]
command=gunicorn --workers 4 --bind unix:/run/gunicorn/socket --access-logfile /var/log/gunicorn/access.log --log-file /var/log/gunicorn/app.log --capture-output simpleapp:app
directory=/path/to/project
user=root
autostart=true
autorestart=true
redirect_stderr=true
{% endhighlight %}

## [Optional] Configuration for logrotate
{% highlight logrotate %}
# simpleapp-logrotate
# copy the config to /etc/logrotate.d
# or create link: $ ln -s /path/to/simpleapp-logrotate /etc/logrotate.d
/var/log/gunicorn/*.log {
    compress
    delaycompress
    missingok
    notifempty
    weekly
}
{% endhighlight %}

## Start service
{% highlight bash %}
$ /etc/init.d/nginx start
$ /etc/init.d/supervisor start
{% endhighlight %}

## References
- [Deploying Gunicorn](http://docs.gunicorn.org/en/stable/deploy.html)
- [Ubuntu Manpage:logrotate ‐ rotates, compresses, and mails system logs](http://manpages.ubuntu.com/manpages/xenial/man8/logrotate.8.html)

## Notes
- The above web stack is deployed with a regular ubuntu linux, but a docker container is also recommended
- The above web stack is deployed with system python, but a python venv is highly recommended
