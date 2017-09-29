---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div style="word-break:break-all;">
{% for group in site.data.bookmarks %}
    {{ group.category }}
    <hr>
    {% for bookmark in group.data %}
    <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.description }}</a>
    {% endfor %}
    <br>
{% endfor %}
</div>
