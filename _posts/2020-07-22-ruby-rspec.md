---
layout: post
title: Ruby rspec
date: 2020-07-22 13:10:00 +0900
tags: ruby
---

RSpec is a popular test framework in ruby.

## To write tests in rspec
{% highlight ruby %}
# the class or module to test
describe Integer do
  # the method to test 
  describe '#+' do
    # the condition to test
    context 'when adding two integers' do
      # the test case itself
      it 'calls the :+ method on the first integer' do
        expect(100.+(200)).to eq 300
      end
    end
  end
end
{% endhighlight %}

## To run rspec tests
- With the `rspec` command tool
{% highlight bash %}
# Save the above a file named "integer_spec.rb" for example
# Make sure rspec is already installed with the ruby version
# cd to /path/to/integer_spec.rb
$ rspec ./integer_spec.rb

# Output sample
.

Finished in 0.00341 seconds (files took 0.12764 seconds to load)
1 example, 0 failures
{% endhighlight %}

- With `ruby` command
{% highlight ruby %}
# 1 - require rspec/autorun in the file that contains tests
require 'rspec/autorun'

describe 1 do
  it 'is < 2' do
    expect(1).to be < 2
  end
end

# 2 - execute the file with ruby
# $ ruby /path/to/test.rb
{% endhighlight %}

## Examples of matchers
{% highlight ruby %}
context 'examples' do
  it 'expects' do
    expect(1.+(1)).to eq 2
    expect(1.+(1)).not_to eq 3
    expect([1, 2]).not_to be_empty
    expect(String.class).to be Class
    expect(2).to be > 1
    expect(2).to be >= 1
    expect(2).to be < 3
    expect(2).to be <= 3
    expect(2).to be_between(1, 3)
    expect('2020-01').to match /\d+{4}-\d+{2}/
    expect(0.3 * 3 + 0.1).to be_within(0.001).of(1.0)
    expect('Hello').to start_with 'He'
    expect('Hello').to end_with 'lo'
    expect({}).to be_instance_of Hash
    expect(1).to be_kind_of Numeric
    expect("ruby").to respond_to :length
    expect(1).to be_truthy
    expect(1 > 0).to be true
    expect(nil).to be_falsy
    expect(1 < 0).to be false
    expect([][0]).to be_nil
    expect{1.length}.to raise_error NoMethodError
    expect{1 + '2'}.to raise_error "String can't be coerced into Integer"
    expect{1 / 0}.to raise_error ZeroDivisionError, 'divided by 0'
    expect({name: 'Andy'}).to have_key :name
    expect([1, 2]).to include 2
    expect(1..10).to cover 5
  end
end
{% endhighlight %}

## References
- [RSpec Expectations](https://rubydoc.info/gems/rspec-expectations/frames)
- [Publisher: RSpec - Relish](https://relishapp.com/rspec/)
- [RSpec documentation](https://rspec.info/documentation/)
- [Better Specs { rspec guidelines with ruby }](http://www.betterspecs.org/)
