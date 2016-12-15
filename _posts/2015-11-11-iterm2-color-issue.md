---
layout: post
title: Iterm2 color issue
date: 2015-11-11 16:51:00 +0800
author: Yuan Jiang
tags: mac
---

An issue found for [iterm2](https://www.iterm2.com) on Mac is that the color of profiles does not apply, e.g. run "ls -l" command you don't see the different colors for directories or files.

Solution:
{% highlight bash %}
# 1 - open ~/.bash_profile and add below two items:
export CLICOLOR=1
export TERM=xterm-256color

# 2 - go to: Preferences > Profiles > Terminal > Report Terminal Type
# 3 - set the type to xterm-256color

{% endhighlight %}

See [Fix iTerm2 color scheme does not get applied](http://minhd.net/fix-iterm2-color-scheme/) for reference.
