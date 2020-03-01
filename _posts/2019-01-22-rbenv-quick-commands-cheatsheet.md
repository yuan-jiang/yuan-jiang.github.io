---
layout: post
title: Rbenv quick commands cheatsheet
date: 2019-01-22 09:30:52 +0900
tags: ruby
---

[rbenv](https://github.com/rbenv/rbenv) is a great tool to manage multiple Ruby versions and here are some of its commonly used commands summarized for quick reference.

## Install rbenv itself and setup
{% highlight bash %}
$ brew install rbenv
$ rbenv init # follow the printed instruction and add below entry to ~/.bash_profile
$ eval "$(rbenv init -)"
{% endhighlight %}

## Install/Uninstall ruby versions
{% highlight bash %}
$ rbenv install -l # list all available versions
$ rbenv install <version> # install a specific version
$ rbenv version # show the current version in use
$ rbenv versions # list all installed versions (can also check ~/.rbenv/versions/)
$ rbenv uninstall <version> # uninstall a specific version
{% endhighlight %}

## Set/Unset ruby versions
{% highlight bash %}
$ rbenv local <version> # will add a .ruby-version file into current directory with version name to override the global version
$ rbenv local --unset # unset and remove the .ruby-version file

$ rbenv global <version> # will write to ~/.rbenv/version as global version but it can be overriden by the local one if set above

$ rbenv local # show the current local ruby version used
$ rbenv global # show the current global ruby version used

$ rbenv local system # set local version to current system Ruby ($PATH)
$ rbenv global system # set global version to current system Ruby ($PATH)

$ rbenv shell <version> # set RBENV_VERSION env to override both local/global version set above
$ rbenv shell --unset # unset the RBENV_VERSION env
{% endhighlight %}

## Run executables with selected ruby version (by `.ruby-version` or global)

  + Help doc
{% highlight bash %}
$ rbenv help exec
Usage: rbenv exec <command> [arg1 arg2...]

Runs an executable by first preparing PATH so that the selected Ruby
version's `bin' directory is at the front.

For example, if the currently selected Ruby version is 1.9.3-p327:
  rbenv exec bundle install

is equivalent to:
  PATH="$RBENV_ROOT/versions/1.9.3-p327/bin:$PATH" bundle install
{% endhighlight %}

  + Install gem to selected ruby version
{% highlight bash %}
$ rbenv exec gem install rails
{% endhighlight %}


See [official docs](https://github.com/rbenv/rbenv#command-reference) for more.

