---
layout: post
title: Quick Steps of Using Python VirtualEnv
date: 2015-11-16 17:10:00 +0800
author: Andy Jiang
---

{% highlight bash %}
$ sudo pip install virtualenv
$ mkdir ~/PythonEnvs/
$ virtualenv ~/PythonEnvs/env1/
$ cd ~/PythonEnvs/env1/
$ ls
  bin include lib
$ source bin/activate
(env1)$ =>now you are in the virtual env
// To leave the virtual env
$ deactivate
{% endhighlight %}
