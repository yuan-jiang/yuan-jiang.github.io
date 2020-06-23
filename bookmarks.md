---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div>
  <ul>
    {% for bookmark in site.bookmarks %}
    <li>
      <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.title }}</a>
    </li>
    {% endfor %}
  </ul>
</div>
