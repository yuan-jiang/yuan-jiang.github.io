---
layout: post
title: Upgrade rails from 5.1 to 5.2
date: 2020-10-22 13:25:00 +0900
tags: rails ruby
---

Upgrading application rails version from `5.1` to `5.2` is much easier than doing [from 4.2 to 5.0](/upgrade-rails-from-4.2-to-5.0) or [from 5.0 to 5.1](/upgrade-rails-from-5.0-to-5.1), there are very few changes to handle and chances are there is no test failure.

## Things to handle
- Add `bootsnap` to `Gemfile`  
Rails 5.2 uses a gem called `bootsnap` to speed up app boot time, when you run `rails app:update`, the `boot.rb` will be updated to have such line:
{% highlight ruby %}
require 'bootsnap/setup' # Speed up boot time by caching expensive operations.
{% endhighlight %}
However, when you run `rails c` or `rails s`, chances are an error will be thrown about the missing of the `bootsnap` gem in `Gemfile`, which is due to that the upgraded version of app does not have the gem included in Gemfile yet. Just add it as follows:
{% highlight ruby %}
# Reduces boot times through caching; required in config/boot.rb
gem 'bootsnap', '>= 1.1.0', require: false
{% endhighlight %}

- Config defaults  
Rails 5.2 introduces a bunch of default configurations which are saved in the `config/initializers/new_framework_defaults_5_2.rb` file and by default commented out.  
It depends on the use cases per specific app to decide whether it is necessary to use the defaults, when in doubt, you can just keep the defaults from previous version and only enable the new config that's necessary. For instance:
{% highlight ruby %}
config.load_defaults 5.1
{% endhighlight %}
and uncomment out any config that is really necessary in the `new_framework_defaults_5_2.rb` file.

## Others
- Rails 5.2 removed `limit: 24` option for `float` type, so you need regenerate a copy of `schema.rb` under rails 5.2 by:
{% highlight bash %}
$ bin/rails db:schema:dump
{% endhighlight %}
- Deprecation warning about `dalli_store`, which can be replaced with official `:mem_cache_store`. You can follow the official documentation for the switch when necessary:
{% highlight text %}
DEPRECATION: :dalli_store will be removed in Dalli 3.0.
Please use Rails' official :mem_cache_store instead.
https://guides.rubyonrails.org/caching_with_rails.html
{% endhighlight %}

## Summary
Based on app features, there might be other things to handle, such as the use of `config/credentials.yml.enc` and `config/master.key` and so on.  
For more detailed changes, always refer to the offical upgrade guide and release notes:
- https://guides.rubyonrails.org/upgrading_ruby_on_rails.html#upgrading-from-rails-5-1-to-rails-5-2
- https://guides.rubyonrails.org/5_2_release_notes.html
