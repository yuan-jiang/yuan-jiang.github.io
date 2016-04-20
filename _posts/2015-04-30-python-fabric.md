---
layout: post
title: Python fabric
date: 2015-04-30 15:24:00 +0800
author: Yuan Jiang
tags: python
---

Basic usage of python fabric.

Installation
{% highlight bash %}
$ sudo pip install fabric

$ which fab
/usr/local/bin/fab (Mac)
{% endhighlight %}

Run commands directly from command line
{% highlight bash %}
$ fab -H 1.1.1.1 -u root -p root123 -- 'uname -a'
# -H => The remote host to run command on, same as --hosts
# -u => The ssh username of the remote host, same as --user
# -p => The ssh password of the remote host, same as --password
# fab -h => list all available options and args
{% endhighlight %}

Run fabfile and passing in command line arguments  

- Define fabfile.py
{% highlight python %}
from fabric.api import run, env, local
env.hosts = []
env.user = "root"
env.password = "root123"
env.warn_only=True

def show_ros_info():
    '''show remote os info'''
    run("uname -a")

def show_los_info():
    '''show local os info'''
    local("uname -a")

# an example to add swap for remote os
def add_swap(swapfile="/opt/swap", swapsize_mb=512):
    count = 1024 * int(swapsize_mb)
    run("free -m")
    run("dd if=/dev/zero of=%s bs=1024 count=%s" % (swapfile, str(count)))
    run("mkswap %s" % swapfile)
    run("chown root:root %s" % swapfile)
    run("chmod 0600 %s" % swapfile)
    run("swapon %s" % swapfile)
    run("echo %s swap swap defaults 0 0 >> /etc/fstab" % swapfile)
    run("free -m")
{% endhighlight %}

- Run above fabfile
{% highlight bash %}
$ fab show_ros_info:hosts=1.1.1.1

$ fab show_los_info:host=1.1.1.2

$ fab add_swap:hosts=1.1.1.1

$ fab add_swap:swapsize_mb=256

$ fab add_swap:hosts=1.1.1.1,swapfile=/tmp/test,swapsize_mb=1024

$ fab -f xxx.py ...
  or
$ fab --fabfile xxx.py ...

# NOTE:
# 1) The option 'hosts' (or host/role/roles) are special keyword arguments for
#    per-task configuration; if absent, the env.hosts will be used.
#
# 2) The function arguments should be passed after the colon(:) and separated
#    with comma(,) and match definition; but optional for keyword arguments.
#
# 3) If the fabfile.py is not named fabfile.py, such as xxx.py instead, then
#    the file must be explicitly provided using the "-f" or "--fabfile" option.
{% endhighlight %}

See [Fabric's documentation](http://docs.fabfile.org/en/1.11/) for more details.
