---
layout: post
title: Selenium css selector
date: 2012-03-30 14:45:00 +0800
author: Yuan Jiang
tags: css selenium webdriver automation
---

Selenium web element locating strategy using css selector.

## Common css selector
- Direct child using ">"
{% highlight css %}
<div>
  <a>...</a>
</div>
css=div > a
{% endhighlight %}

- Child or subchild using whitespace
{% highlight css %}
<div>
  ...
    ...
      <a>...</a>
    ...
  ...
</div>
css=div a
{% endhighlight %}

- Id using "#"
{% highlight css %}
<div id='foo'>
  ...
    <a>...</a>
  ...
</div>
css=div#foo a
{% endhighlight %}

- Class using "."
{% highlight css %}
<div class="foo">
  ...
    <a>...</a>
  ...
</div>
css=div.foo a
{% endhighlight %}

- All elements using "*"
{% highlight css %}
<div id="foo">
  ...
</div>
css=div#foo > *
{% endhighlight %}


- Next sibling using "+"
{% highlight css %}
<form>
  <input id="username"></input>
  <input>...</input>
</form>
css=input#username + input
{% endhighlight %}

- Following sibling using "~"
{% highlight css %}
<form>
  <input id="username"></input>
  ...
  <input>...</input>
</form>
css=input#username ~ input
{% endhighlight %}


- Attribute values using key=value
{% highlight css %}
<form>
  <input name="username" type="text"></input>
  <input name="submit" type="button"></input>
</form>
css=input[name='username']
css=input[name='submit'][type='button']
{% endhighlight %}

- Index of child using nth-xxx(n)
{% highlight css %}
css=ul#foo li:nth-of-type(3)
css=ul#foo li:first-of-type
css=ul#foo li:last-of-type
css=ul#foo li:nth-last-of-type(3)
css=ul#foo li:nth-last-child(3)
css=ul#foo li:nth-child(3)
css=ul#foo *:nth-child(3)
css=ul#foo li:first-child
css=ul#foo li:last-child
css=ul#foo li:only-child
css=ul#foo li:only-of-type
css=ul#foo li:empty
css=ul#foo li:nth(2) # sizzle
{% endhighlight %}

- Sub-string using special chars
{% highlight css %}
1) start with:
css=div[id^='prefix']
2) ends with:
css=div[id$='suffix']
3) contains:
css=div[id*='substring'] #containing substring
css=div[id~='word']      #containing word
{% endhighlight %}

- Inner text using contains
{% highlight css %}
<div>
  <a>Log Out!</a>
</div>
css=div a:contains('Log Out') # sizzle
{% endhighlight %}

- Negative using not (sizzle)
{% highlight css %}
css=a.confirmation_link:not(.hidden)
css=.confirmation_link:not(div)
css=.submit_button:not(#clear_button)
css=input[type=button]:not(p#not_this_input > input)
{% endhighlight %}

## References
- [SELENIUM TIPS: CSS SELECTORS](http://saucelabs.com/resources/selenium/css-selectors)
- [CSS Selectors Reference](http://www.w3schools.com/cssref/css_selectors.asp)
