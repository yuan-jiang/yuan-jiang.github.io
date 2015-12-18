---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div style="word-break:break-all;">
{% for bookmark in site.data.bookmarks %}
    <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.description }}</a>
{% endfor %}
</div>
