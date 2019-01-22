---
layout: post
title: Nodenv quick commands cheatsheet
date: 2018-12-12 17:17:52 +0900
tags: node js
---

[nodenv](https://github.com/nodenv/nodenv) is a great tool to manage multiple NodeJS versions and here are some of its commonly used commands summarized for quick reference.

## Install nodenv itself and setup
{% highlight bash %}
$ brew install nodenv
$ nodenv init # follow the printed instruction and add below entry to ~/.bash_profile
$ eval "$(nodenv init -)"
{% endhighlight %}

## Install/Uninstall node versions
{% highlight bash %}
$ nodenv install -l # list all available versions
$ nodenv install <version> # install a specific version
$ nodenv versions # list all installed versions (can also check ~/.nodenv/versions/)
$ nodenv uninstall <version> # uninstall a specific version
{% endhighlight %}

## Set/Unset node versions
{% highlight bash %}
$ nodenv local <version> # will add a .node-version file into current directory with version name to override the global version
$ nodenv local --unset # unset and remove the .node-version file

$ nodenv global <version> # will write to ~/.nodenv/version as global version but it can be overriden by the local one if set above

$ nodenv local # show the current local node version used
$ nodenv global # show the current global node version used

$ nodenv local system # set local version to current system Node ($PATH)
$ nodenv global system # set global version to current system Node ($PATH)

$ nodenv shell <version> # set NODENV_VERSION env to override both local/global version set above
$ nodenv shell --unset # unset the NODENV_VERSION env
{% endhighlight %}


See [official docs](https://github.com/nodenv/nodenv#command-reference) for more.

