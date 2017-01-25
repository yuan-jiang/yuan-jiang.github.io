---
layout: post
title: SSL error with python requests
date: 2017-01-25 17:22:00 +0800
author: Yuan Jiang
tags: python ssl
---

Quick fix to an ssl error was encountered with python requests on ubuntu 14.04.

## Error message
{% highlight text %}
  File "/usr/lib/python2.7/dist-packages/requests/api.py", line 55, in get
    return request('get', url, **kwargs)
  File "/usr/lib/python2.7/dist-packages/requests/api.py", line 44, in request
    return session.request(method=method, url=url, **kwargs)
  File "/usr/lib/python2.7/dist-packages/requests/sessions.py", line 455, in request
    resp = self.send(prep, **send_kwargs)
  File "/usr/lib/python2.7/dist-packages/requests/sessions.py", line 558, in send
    r = adapter.send(request, **kwargs)
  File "/usr/lib/python2.7/dist-packages/requests/adapters.py", line 385, in send
    raise SSLError(e)
requests.exceptions.SSLError: [Errno 1] _ssl.c:510: error:14077438:SSL routines:SSL23_GET_SERVER_HELLO:tlsv1 alert internal error
{% endhighlight %}

## Solution
{% highlight bash %}
$ sudo apt-get install libffi-dev
$ pip install pyOpenSSL ndg-httpsclient pyasn1
{% endhighlight %}

## References
- [https GET request fails with "handshake failure" #2022](https://github.com/kennethreitz/requests/issues/2022)
- [SSL Error 14077438 #2604](https://github.com/kennethreitz/requests/issues/2604)
