---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div style="word-break:break-all;">
{% for category in site.data.bookmarks %}
    [{{ category }}]
    <hr>
    {% for bookmark in site.data.bookmarks[category] %}
    {% for bookmark in category %}
    <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.description }}</a>
    {% endfor %}
{% endfor %}
</div>
