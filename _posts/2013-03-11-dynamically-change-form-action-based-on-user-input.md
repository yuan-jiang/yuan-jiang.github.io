---
layout: post
title: Dynamically change form action based on user input
author: Yuan Jiang
date: 2013-03-11 15:40:00 +0800
tags: html
---

How to dynamically change html form action value based on user input.

{% highlight html %}
<span id="search">

    <form name="s" role="search" method="get" action="/search" onsubmit="return determineAction()">
        <input id="searchString" name="q" placeholder="Enter text to search..." type="text"></input>
        <input id="searchButton" name="submit" type="submit" value="GO"></input>
    </form>

    <script type="text/javascript">
      function determineAction()
      {
        if (document.s.q.value != "")
        {
          document.s.action = "https://www.google.com/search";
        }
        return true;
      }
    </script>

</span>
{% endhighlight %}
