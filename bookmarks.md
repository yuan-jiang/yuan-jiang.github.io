---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div>
{% for group in site.data.bookmarks %}
    <span style="font-style: italic; font-weight: bold;">{{ group.category }}</span>
    <hr style="border-top: dotted 1px;" />
    {% for bookmark in group.data %}
    <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.description }}</a>
    {% endfor %}
    <br>
{% endfor %}
</div>
