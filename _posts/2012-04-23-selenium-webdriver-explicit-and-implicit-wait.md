---
layout: post
title: Selenium webdriver explicit and implicit wait
date: 2012-04-23 13:38:00 +0800
author: Yuan Jiang
tags: selenium webdriver
---

Selenium webdriver explicit wait and implicit wait.

Implicit wait
{% highlight python %}
ff = webdriver.Firefox()
ff.implicitly_wait(10) # seconds
ff.get("http://www.google.com")
element = ff.find_element_by_id("element_id")
{% endhighlight %}

Explicit wait
{% highlight python %}
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait # available since 2.4.0
from selenium.webdriver.support import expected_conditions as EC # available since 2.26.0

ff = webdriver.Firefox()
ff.get("http://www.google.com")
try:
    element = WebDriverWait(ff, 10).until(EC.presence_of_element_located((By.ID, "element_id")))
finally:
    ff.quit()
{% endhighlight %}
