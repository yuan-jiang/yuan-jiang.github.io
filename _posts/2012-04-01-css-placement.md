---
layout: post
title: CSS placement
date: 2012-04-01 13:49:00 +0800
author: Yuan Jiang
tags: html css
---

Three kinds of css placement in HTML.

## External css
{% highlight html %}
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
{% endhighlight %}

## Internal css
{% highlight html %}
<head>
<style type="text/css">
hr {color:red;}
p {margin-left:20px;}
body {background-image:url("images/backimg.gif");}
</style>
</head>
{% endhighlight %}

## Inline css
{% highlight html %}
<p style="color:black; margin-left:20px;">This is a paragraph.</p>
{% endhighlight %}

## References
- [CSS Tutorial](http://www.w3schools.com/css/default.asp)
- [CSS Reference](http://www.w3schools.com/cssref/default.asp)
