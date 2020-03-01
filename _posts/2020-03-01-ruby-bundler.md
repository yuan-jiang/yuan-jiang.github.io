---
layout: post
title: Ruby bundler
date: 2020-03-01 23:20:00 +0900
tags: ruby
---

Bundler is for dependency management in ruby projects.

## Bundler is itself a ruby gem, so you need install it first
{% highlight bash %}
$ gem install bundler
{% endhighlight %}

## Prepare `Gemfile` file in project root directory
- Manually create it;
- Run `bundle init` to automatically create it;

## Specify dependencies in `Gemfile`
{% highlight ruby %}
source 'https://rubygems.org'
gem 'rails', '~> 4.2.0'
gem 'rspec'
gem 'puma'
{% endhighlight %}
Refer to [Gemfiles](https://bundler.io/v2.0/gemfile.html) for in-depth.

## Install dependencies based on `Gemfile`
{% highlight bash %}
$ bundle install
{% endhighlight %}
After installation completes, it will also generate `Gemfile.lock` with snapshot of depencency details. See [more](https://bundler.io/v2.0/bundle_install.html).

## Setup in application
{% highlight ruby %}
require 'rubygems'  # builtin for ruby 1.9+ so no need
require 'bundler/setup'

# option 1 - require dependency one by one if not many, e.g.
require 'rails'
require 'rspec'

# option 2 - require all by groups if many to avoid requiring one by one, e.g.
Bundler.require(:default)
{% endhighlight %}
See [more](https://bundler.io/v2.0/guides/bundler_setup.html).

## Execute a command in the context of the bundle
{% highlight bash %}
$ bundle exec rspec ./spec/model_spec.rb
{% endhighlight %}


## References
- [Bundler Docs](https://bundler.io/docs.html)