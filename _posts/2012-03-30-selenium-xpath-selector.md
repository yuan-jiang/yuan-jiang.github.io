---
layout: post
title: Selenium xpath selector
date: 2012-03-30 16:24:00 +0800
author: Yuan Jiang
tags: selenium webdriver xpath automation
---

Selenium web element locating strategy using xpath.

## Common xpath selector
- Absolute reference and child reference with "/"
{% highlight xml %}
xpath=/html  # starting from root
xpath=./div  # child div of current element
xpath=/html/body
{% endhighlight %}

- Relative reference and descendant reference with "//"
{% highlight xml %}
xpath=//input    # any input in whole dom
xpath=.//input   # any input from current location
xpath=//div//input # input is descendant of div
xpath=//div/input  # input is child of div
{% endhighlight %}

- Parent element with parent:: and ..
{% highlight xml %}
xpath=//div[@id='foo']/parent::div
xpath=//div[@id='foo']/../../div
{% endhighlight %}

- Any element and any attribute
{% highlight xml %}
xpath=//div/input[@type='text']/*
xpath=//*[text()='foo']
xpath=//div[@*='foo']
{% endhighlight %}

- Attribute and attribute value with "@"
{% highlight xml %}
1) has attribute:
xpath=//div[@attr]
2) attribute value match:
xpath=//div[@attr='foo']
3) attribute value contains:
xpath=//div[contains(@attr, 'foo')]
4) attribute value starts with:
xpath=//div[starts-with(@attr, 'foo')]
NOTE: ends-with not supported.
{% endhighlight %}

- Multiple attributes or conditions
{% highlight xml %}
xpath=//div[@attrA='foo' and @attrB='bar']
xpath=//div[@attrA='foo'][@attrB='bar']
xpath=//div[starts-with(@attrA, 'foo')][contains(@attrB, 'bar')]
xpath=//div[starts-with(@attrA, 'foo') or contains(@attrB, 'bar')]
xpath=//div[starts-with(@attrA, 'foo') and contains(@attrB, 'bar')]
{% endhighlight %}

- Inner text with text()
{% highlight xml %}
xpath=//div[text()='foobar']
xpath=//div[contains(text(), 'foo')]
{% endhighlight %}

- Index with [] and position()
{% highlight xml %}
xpath=//input[n]
xpath=//input[last()]
xpath=//input[last()-1]
xpath=//div/div[position()=2]
{% endhighlight %}

- Child count with count(*)
{% highlight xml %}
xpath=//div[count(*)=0]  # no child
xpath=//div[count(*)=1]  # only 1 child
xpath=//div[count(*)>2]  # more than 2 children
{% endhighlight %}

- Sibling
{% highlight xml %}
xpath=//div[@id='foo']/div[@class]/following-sibling::div
xpath=//div[@id='foo']/div[@class]/preceding-sibling::div
{% endhighlight %}

- Nesting condition
{% highlight xml %}
xpath=//div[@id='foo']/div[p]  # has p child
xpath=//div[@id='foo']/div[following-sibling::div[@id='bar']]
xpath=//div[@id='foo']//div[div[@class='bar']]
{% endhighlight %}

## References
- [XPath Wikipedia](https://zh.wikipedia.org/zh-cn/XPath)
- [XPath Syntax](http://www.w3schools.com/xsl/xpath_syntax.asp)
