---
layout: post
title: Linux bash scripting basics and tips
date: 2014-12-25 18:35:00 +0800
author: Yuan Jiang
tags: linux bash
---

Linux bash scripting basics and tips.

Shebang
{% highlight bash %}
$ #!/usr/bin/env bash => better portability
or
$ #!/bin/bash
{% endhighlight %}

Comments and quotes
{% highlight bash %}
$ # This is a comment line
$ echo '`date`' => print `date` literally
$ echo "`date`" => print actually date string
{% endhighlight %}

Debugging
{% highlight bash %}
$ #!/usr/bin/env bash -x
#or
$ #!/bin/bash -x
#or
$ bash -x script.sh
#or within script
$ set -x => start debugging
$ set +x => stop debugging
{% endhighlight %}

Variables
{% highlight bash %}
$ printenv # display global env
#or
$ env # display global env
$ set # display local env
$ var=value # define local var in current shell
# notice there is no space between the equal sign
$ export var=value # export var and available in sub shell
# special variables
$ $? # Exit status of most recently executed command
$ $0 # Current script name
$ $# # Number of parameters
$ $@ # Positional parameters array
$ $x # Positional parameter: x = 1, 2, 3, ...
{% endhighlight %}

Execute script in current shell
{% highlight bash %}
$ source script.sh
#or
$ . script.sh # notice the dot (.) command
{% endhighlight %}

Shell expansion
{% highlight bash %}
$ echo sp{el,il,al}l # spell spill spall
$ ~ # $HOME

$ $VAR # get var value
#or
$ ${VAR} # get var value

$ $(command) # get command output
#or
$ `command` # get command output

$ $((expression)) # get arithmetic expression value
#or
$ $[expression] # get arithmetic expression value
{% endhighlight %}


## Reference links
- [BASH Programming - Introduction HOW-TO
](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)
- [Bash Guide for Beginners
](http://www.tldp.org/LDP/Bash-Beginners-Guide/html/index.html)
- [Advanced Bash Scripting](http://www.tldp.org/LDP/abs/abs-guide.pdf)
