---
layout: post
title: Ruby basics
date: 2015-11-18 14:35:00 +0800
author: Andy Jiang
tags: ruby
---

Learn some basics of Ruby

{% highlight ruby %}
# The Greeter class
class Greeter
  def initialize(name)
    @name = name.capitalize
  end

  def salute
    puts "Hello #{@name}!"
  end
end
# Create a new object
g = Greeter.new("world")
# Output "Hello World!"
g.salute
{% endhighlight %}


Ruby Iteration
{% highlight ruby %}
some_list.each do |this_item|
  # We're inside the block.
  # deal with this_item.
end

some_list.each {|item| puts "I got #{item}" }
{% endhighlight %}

Variable naming convention
{% highlight ruby %}
Starts with     |  Variable type
----------------|---------------
Capital letter  |  Constant
      $         |  Global
      @         |  Instance
     @@         |  Class
{% endhighlight %}

Embed variable inside a string
{% highlight ruby %}
  greeting = "Hello #{name}" # must be double quotes, if single quotes, it's ignored
{% endhighlight %}

Get user input
{% highlight ruby %}
gets.chomp  => Get user input from command line using keyboard
            => in = $stdin.gets.chomp
            => gets.chomp.to_i
ARGV        => Get user input from command line via script argument
            => filename = ARGV.first
            => first, second, third = ARGV
{% endhighlight %}

Ruby heredoc - multi-line string
{% highlight ruby %}
<<WORD
This is line 1
This is line 2
...
This is line N
WORD
=> Can use any all caps word
{% endhighlight %}
