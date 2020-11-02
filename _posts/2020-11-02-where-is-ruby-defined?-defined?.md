---
layout: post
title: Where is ruby defined? defined?
date: 2020-11-02 17:07:00 +0900
tags: ruby
---

Ever wonder where ruby's `defined?` is defined? Because it looks so like a method, you might end up trying to check out a bunch of places, such as: `Object`, `Kernel`, `BasicObject`, or even `Class` or `Module`, but only to find no luck locating it in any of such places. Well, only until you've realized it, the answer is pretty simple: `defined?` is not a method but an operator or a keyword.

## How to use `defined?`?
{% highlight ruby %}
irb(main):001:0> defined? String
=> "constant"
irb(main):002:0> defined?(String)
=> "constant"
{% endhighlight %}

## What does `defined?` return?
{% highlight ruby %}
irb(main):001:0> defined? 1
=> "expression"
irb(main):002:0> defined? dummy
=> nil
irb(main):003:0> defined? printf
=> "method"
irb(main):004:0> defined? String
=> "constant"
irb(main):005:0> defined? $_
=> "global-variable"
irb(main):006:0> defined? true
=> "true"
irb(main):007:0> defined? false
=> "false"
irb(main):008:0> defined? nil
=> "nil"
irb(main):009:0> defined? RUBY_ENGINE
=> "constant"
irb(main):010:0> defined? (a, b = 1, 2)
=> "assignment"
# ...
{% endhighlight %}

## References
- [Programming Ruby: The Pragmatic Programmer's Guide](http://ruby-doc.com/docs/ProgrammingRuby/html/tut_expressions.html#UG)
- [The defined? keyword in Ruby](https://medium.com/rubycademy/the-defined-keyword-in-ruby-b7a5a5a48e1e)
