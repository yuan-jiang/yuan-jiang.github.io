---
layout: post
title: Rails autoload thread unsafe
date: 2020-06-24 12:45:00+0900
tags: rails ruby
---

Rails framework autoloads constants so that in environment like `development` or `test`, the app does not need to load everything into memory before serving requests. However, autoloading is known to be thread unsafe in rails (at least for version less than 6), and if you happen to be using threaded servers like `puma` or `webrick`, it's very easy to step on unexpected issues caused by multiple threads.


## Issue
  > Background: recently after I upgraded our rails API server to ruby 2.6.x series, an autoloading related error has been observed from time to time when `some` of the pages are accessed in development environment.

`Unable to autoload constant {SOME_CONSTANT}, expected {/path/to/some_constant.rb} to define it`

## Investigation
  - Most of the resouces you can get from Google or Stackoverflow seem to be pointing that the root cause lies in:
    + Typo in file directories or names
    + Nesting namespace not matching the directory structure
    + Wront records added to `autoload_paths` in configuration 
    +  etc.
  However, after manually checking each of the constant files reported in the above errors, I could confirm that there was no such problem in our code.

  - Further analysis about the configuration and the web server we use, a sudden clue came to my mind...threading problem, because:
    + The configuration for `development.rb` is having the following values for:
      * `config.cache_classes = false`
      * `config.eager_load = false`
      * `config.autoload_paths << ...`
      and we are using the default `webrick` server. (By the way, the rails version is `5.0.7.2` for which webrick is multi-threaded, see [reference](https://gist.github.com/yob/04c2417b60532316685123c36ddfce40))
    + If it is threading problem related to autuloading, then it must happen to any api endpoint, so I wrote a simple script and:
      * Start app with `webrick`, the issue happened with the simple API endpoint;
      * Start app with `puma` with threads count set to greater than 1, the issue still existed;
      * Start app with `unicorn` with default config, the issue was gone.
{% highlight ruby %}
require 'net/http'

# Adjust the number when necessary
THREADS_COUNT = 10

# Use the same url endpoint to make sure
# multi-requests are sent to and hit the
# same controller#action, or rather the
# same constant that rails is going to
# autoload -> which is thread unsafe!!!
API_ENDPOINT = 'http://localhost:3000/api/environment'

threads = []

THREADS_COUNT.times do
  threads << Thread.new do
    res = Net::HTTP.get(URI.parse(API_ENDPOINT))
    puts "thread=#{Thread.current.object_id}: #{res}"
  end
end

threads.each { |t| t.join }

puts 'DONE'
{% endhighlight %}

      > Conclusion: the issue was not because of some typo or namespace problem, 
        but caused by that `autoloading in rails is thread unsafe` which is a fact.

## Solution
Switch to `unicorn` for development environment as well (same as production environment), which was made easy by a gem called `unicorn-rails`

## Summary
- The error message was a little bit misleading in the first place because it seemed to a cause of typo or namespace or directory problems, but indeed it was caused due to multiple threads trying to autoload the exact same constant, therefore causing race conditions
- Starting rails 6, a new loader called Zeitwerk has been introduced, which seemed to have addressed the thread unsafe issue
- Ruby has autoload method that is discouraged for use due to threading problem
- In rails configuration and environment, the following items are important and usually tends to be the source of such issues, so you must be careful when tuning the configuration:
  + `config.cache_classes`
  + `config.eager_load`
  + `config.autoload_paths`
  + `config.eager_load_paths`
- For troubleshooting the missing constants issues that might be caused really by typo or namespace or directory, you could:
  + Check the `ActiveSupport::Dependencies.autoload_paths` in rails console
  + Check the `ActiveSupport::Dependencies.autoloaded_constants` in rails console
- Read the rails guide section about [Autoloading and Reloading Constants](https://guides.rubyonrails.org/autoloading_and_reloading_constants_classic_mode.html) again and again

## References
- [Autoloading and Reloading Constants (Classic Mode)](https://guides.rubyonrails.org/autoloading_and_reloading_constants_classic_mode.html)
- [Rails 5 disables autoloading while app in production](https://blog.bigbinary.com/2016/08/29/rails-5-disables-autoloading-after-booting-the-app-in-production.html)
- [Bug #921: autoload is not thread-safe - Ruby master - Ruby Issue Tracking System](https://bugs.ruby-lang.org/issues/921)
- [Rails falls back on non-thread-safe autoloading even when eager_load is true](https://github.com/rails/rails/issues/13142)
- [How rails resolve multi-requests at the same time?](https://stackoverflow.com/questions/14027151/how-rails-resolve-multi-requests-at-the-same-time)
- [Feature #5653: "I strongly discourage the use of autoload in any standard libraries" (Re: autoload will be dead) - Ruby master - Ruby Issue Tracking System](https://bugs.ruby-lang.org/issues/5653)
- [Understanding autoload_paths and namespaces in Ruby on Rails](https://www.fatlemon.co.uk/2019/07/understanding-autoload-paths-and-namespaces-in-ruby-on-rails/)
