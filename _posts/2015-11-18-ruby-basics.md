---
layout: post
title: Ruby basics
date: 2015-11-18 14:35:00 +0800
author: Yuan Jiang
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


Iteration
{% highlight ruby %}
some_list.each do |this_item|
  # We're inside the block.
  # deal with this_item.
end

some_list.each {|item| puts "I got #{item}" }

some_hash.each {|key, value|
  # deal with key or value
}
{% endhighlight %}

Variable naming convention
{% highlight ruby %}
Starts with     |  Variable type
----------------|---------------
Lower letter    |  Local
      _         |  Local
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

Heredoc - multi-line string
{% highlight ruby %}
<<WORD
This is line 1
This is line 2
...
This is line N
WORD
=> Can use any all caps word
{% endhighlight %}

Regular expression
{% highlight ruby %}
/pattern/ =~ target_string
/pattern/i =~ target_string #=> case insensitive
{% endhighlight %}

Conditionals
{% highlight ruby %}
if condition then
  statement
end

if condition1 then
  statement1
elsif condition2 then
  statement2
else
  statement3
end

unless condition then
  statement
end

unless condition then
  statement1
else
  statement2
end

case VAR
when value1 then
  statement1
when value2 then
  statement2
when value3, valueX then
  statement3
else
  statement4
end
 #=> when can be: value, type or regexp
{% endhighlight %}

Loop
{% highlight ruby %}
N.times do
  #repeatable operations
end

N.times {
  #repeatable operations
}

N.times { |i|
  #repeatable operations
  #the i-th time
}

for VAR in startX..endX do
  #repeatable operations
end

for VAR in object do
  #repeatable operations
end

while condition do
  #repeatable operations
end

until condition do
  #repeatable operations
end

some_object.each do |VAR|
  #repeatable operations
end

some_object.each {|VAR|
  #repeatable operations
}

loop {
  #repeatable operations
}

break  #=> stop loop
next   #=> jump to next
redo   #=> repeat current
{% endhighlight %}

Equality comparison
{% highlight ruby %}
==        #=> equal value
equal?    #=> same object
eql?      #=> same value
===       #=> based on what to compare on the left side:
          #=> 1) numeric or string, same as ==
          #=> 2) regexp, same as =~
          #=> 3) class, whether is instance
{% endhighlight %}

Class
{% highlight ruby %}
class HelloWorld
  attr_accessor :name
  ..
end #=> define a getter and setter methods to instance variable "name"

class HelloWorld
  def HelloWorld.hello(name)
    print "Hello, ", name
  end
end #=> way 1 to define class method

class HelloWorld
..
end
class << HelloWorld
  def hello(name)
    print "Hello, ", name
  end
end #=> way 2 to define class method

class HelloWorld
  def self.hello(name)
    print "Hello, ", name
  end
end #=> way 3 to define class method

HelloWorld.hello  #=> way 1 to access class method
HelloWorld::hello #=> way 2 to access class method
HelloWorld::XXX   #=> access class constant
{% endhighlight %}

Module
{% highlight ruby %}
module HelloModule
  Version = "1.0"   # module constant
  def hello(name)
    print "Hello, ", name
  end
  module_function :hello  # make hello a public module function
end

HelloModule::Version # access module constant
HelloModule::hello("Ruby")  # call module public function
{% endhighlight %}

Exception handling
{% highlight ruby %}
begin
  # code block that may have exceptions
rescue
  # code block that handle exceptions
end

begin
  # code block that may have exceptions
rescue => VAR
  # code block that handle exceptions
end

begin
  # code block that may have exceptions
rescue Exception1 => ex1
  # code block that handle Exception1
rescue Exception2, Exception3 => ex2
  # code block that handle Exception2 and Exception3
rescue => ex
  # code block that handle StandardError and its sub exceptions
end

special VAR
$!  => most recent exception
$@  => trace of most recent exception (=>$!.backtrace)

begin
  # code block that may have exceptions
rescue => ex
  # code block that handle exceptions
ensure
  # code block that always run regardless exceptions
end

raise "message"                   #=> raise RuntimeError with specified message
raise CustomException             #=> raise custom exception
raise CustomException, "message"  #=> raise custom exception with specified message
raise                             #=> 1) within rescue, raise $! (most recent exception again)
                                  #=> 2) outside rescue, raise RuntimeError
{% endhighlight %}
