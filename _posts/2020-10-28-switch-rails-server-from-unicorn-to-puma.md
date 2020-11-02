---
layout: post
title: Switch rails server from unicorn to puma
date: 2020-10-28 15:45:00 +0900
tags: rails ruby
---

In rails apps among the commonly-used two popular web servers, [unicorn](https://yhbt.net/unicorn/) serves requests with worker processes, while [puma](https://puma.io/) can do it with both process workers and threads. Therefore, switching to puma from unicorn can not only help improve concurrency but also reduce memory usage. Plus, since rails 5.0, the default web server used is puma.

## Prerequisites
In order to use puma for rails app, we must make sure the code is thread safe. Although the way how rails framework was designed to serve request by instantializing a new controller object per request makes it safe under multiple threads environment, there are still many things to check and update, dependent on specific apps.

- Check the `lib` folder  
The `lib` folder usually contains code used by adhoc scripts or batch jobs that is not used by MVC code, but legacy apps tend to add the `lib` folder or its subfolders into `autoload_paths` with eager loading and cache class turned off (in our case), so we need refactor them first to:
  + Move code in `lib` that is used by MVC code to `app/lib`
  + Leave only adhoc script related code in the `lib` folder

- Check configuration about eager loading and cache classes  
Set the values based on recommendation:
{% highlight ruby %}
# for production/preprod
config.eager_load = true
config.cache_classes = true

# for development
config.eager_load = false
config.cache_classes = false

# for test
config.eager_load = false
config.cache_classes = true

# For all environments
# Do not change or mess with autoload_paths because it's not necessary
{% endhighlight %}

- Search for `@@` about class variables usage  
Search for class variables in code base, and even if they are read only after app finishes loading, most of the time, they should not be used and can be replaced by constants.

- Search for `||=` about memoization usage on class instance variables  
The `||=` on class instance variables should be avoided, such as:
{% highlight ruby %}
class << self
  def config
    @config ||= load_config
  end
end
{% endhighlight %}
should be guarded with a mutex block like:
{% highlight ruby %}
@mutex = Mutex.new

class << self
  def config
    @config ||= @mutex.synchronize do
      @config ||= load_config
    end
  end
end
{% endhighlight %}

- Maybe some others depending on app...

## Switch to use puma for default server
- Update `Gemfile`  
For legacy app using unicorn, the `Gemfile` and `Gemfile.lock` need updated:
{% highlight ruby %}
gem 'unicorn' # remove this
gem 'puma' # add this
{% endhighlight %}
and run `bundle i` to update `Gemfile.lock`

- Prepare environment specific puma configuration  
Create `config/puma/{environment}.rb` config files to adjust config based on environment, for example:

{% highlight ruby %}
port        ENV.fetch('PORT') { 3000 }
environment ENV.fetch('RAILS_ENV') { 'development' }
{% endhighlight %}

{% highlight ruby %}
bind "unix://#{shared_path}/tmp/sockets/puma.sock"
environment ENV.fetch('RAILS_ENV') { 'production' }
{% endhighlight %}

- Update deploy scripts to deploy rails app with puma instead of unicorn  
If you use [capistrano](https://rubygems.org/gems/capistrano) for deployment, a gem called [capistrano3-puma](https://rubygems.org/gems/capistrano3-puma) makes it easy to deploy with puma but it can also be problematic, such as the latest version of puma (5.0) is not yet supported as of this writing. In such case, you can write custom script to use tools like `systemd` to manage the puma services on linux servers.

## References
* https://github.com/rails/rails/issues/33209
* https://stackoverflow.com/questions/15184338/how-to-know-what-is-not-thread-safe-in-ruby/15184752#15184752
* https://devcenter.heroku.com/articles/deploying-rails-applications-with-the-puma-web-server#thread-safety
* https://bearmetal.eu/theden/how-do-i-know-whether-my-rails-app-is-thread-safe-or-not/
* https://guides.rubyonrails.org/v5.1/autoloading_and_reloading_constants.html
* https://github.com/puma/puma/blob/master/docs/deployment.md
* https://medium.com/@anilkumarmaurya/when-not-to-use-memoization-in-ruby-on-rails-9d54bce0ae74
* https://github.com/rails/rails/pull/9789
* https://blog.arkency.com/3-ways-to-make-your-ruby-object-thread-safe/
* https://stackoverflow.com/questions/62458471/is-establish-connection-on-worker-boot-still-required-on-rails-6-and-puma
* https://github.com/puma/puma/issues/1001
