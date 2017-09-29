---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div style="word-break:break-all;">
{% for category in site.data.bookmarks %}
    [{{ category }}]
    <hr>
<<<<<<< HEAD
    {% for bookmark in site.data.bookmarks[category] %}
=======
    {% for bookmark in category %}
>>>>>>> ff763cd216925152ffc8634470640f80d5d65fd6
    <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.description }}</a>
    {% endfor %}
{% endfor %}
</div>
