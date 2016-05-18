---
layout: post
title: Highlight web element in selenium webdriver
date: 2012-07-03 14:23:00 +0800
author: Yuan Jiang
tags: selenium webdriver
---

How to highlight a web element after it is located in selenium webdriver.

{% highlight java %}
/**
 * Highlight web element found, may slow down test execution a little bit.
 * But is helpful for debugging purpose such as tracking current focus
 * @param element The WebElement to highlight
 */
private void highlight(WebElement element)
{
  final int wait = 1000;
  String originalStyle = element.getAttribute("style");
  try
  {
    setAttribute(element, "style",
        "color: ; border: 0px ; background-color: yellow;");

    Thread.sleep(wait); // highlight for given period

    setAttribute(element, "style",
        "color: ; border: 0px ; background-color: ;");

    setAttribute(element, "style", originalStyle);

  }
  catch (InterruptedException e)
  {
    logger.error(e);  
  }
}

/**
 * Set an attribute to an element in the HTML of a page.
 * @param element The web element to set attribute to
 * @param attributeName The attribute to modify/set
 * @param value The value to set
 */
private void setAttribute(WebElement element, String attributeName, String value)
{
  JavascriptExecutor js = (JavascriptExecutor) this.driver;
  js.executeScript("arguments[0].setAttribute(arguments[1], arguments[2])",
                   element, attributeName, value);
}
{% endhighlight %}
