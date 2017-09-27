---
layout: post
title: Floating horizontal scrollbar
date: 2017-09-27 14:23:00 +0800
tags: jquery css javascript html
---

How to use a jquery plugin to implement floating horizontal scrollbar, which is especially useful for single page application where partial content of the page (i.g. table of data) are large and dynamically updated and you don't want to make use of the browser scrollbar for scrolling the partial content.

## Installation
- Download both css/js files
    + [jquery.floatingscroll.css](https://raw.githubusercontent.com/Amphiluke/floating-scroll/master/src/jquery.floatingscroll.css)
    + [jquery.floatingscroll.js](https://raw.githubusercontent.com/Amphiluke/floating-scroll/master/src/jquery.floatingscroll.js)

- Add both resource files to html document
{% highlight html %}
<link href="/path/to/jquery.floatingscroll.css" rel="stylesheet" type="text/css" />
<script src="/path/to/jquery.floatingscroll.js" type="text/javascript"></script>
{% endhighlight %}


## Create css style for container to have scrollbar
{% highlight css %}
.spacious-container {
    overflow: auto;
    width: 100%;
}
{% endhighlight %}

## Floating scrollbar application - static container
- Apply style to container (e.g. div) for which floating scrollbar will be needed
{% highlight html %}
<div class="spacious-container">
    <table>
        <!-- static table whose data/row/column will not change once loaded -->
    </table>
</div>
{% endhighlight %}

- Apply javascript to initialize floating scrollbar
{% highlight javascript %}
$(document).ready(function () {
    $(".spacious-container").floatingScroll();
});
{% endhighlight %}

## Floating scrollbar application - dynamic container
- Apply style to container (e.g. div) for which floating scrollbar will be needed
{% highlight html %}
<div class="table-controls">
    <button id="refresh-button">Refresh</button>
    <input id="tableColumnToggle" type="checkbox"></input><label>Show all columns</label>
</div>
<div class="spacious-container">
    <table>
        <!-- dynamic table whose data/row/column will be changed (e.g. ajax) based on some events -->
    </table>
</div>
{% endhighlight %}

- Apply javascript to initialize & update floating scrollbar
{% highlight javascript %}
// init floating scrollbar
$(".spacious-container").floatingScroll("init");

// update scrollbar if user clicks on the refresh button to update table data
$("#refresh-button").click(function () {
    $(".spacious-container").floatingScroll("update");
});

// update scrollbar if table columns change (e.g. toggle show/hide)
$("#tableColumnToggle").bind("change", function() {
    $(".spacious-container").floatingScroll("update");
})
{% endhighlight %}

## References
- [Floating-scroll source code](https://github.com/Amphiluke/floating-scroll)
- [Usage examples](http://amphiluke.github.io/jquery-plugins/floatingscroll/)
- [Possible to have a floating horizontal scrollbar?](https://stackoverflow.com/questions/24552684/possible-to-have-a-floating-horizontal-scrollbar)