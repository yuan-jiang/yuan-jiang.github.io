---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

<div>
  {% for group in site.data.bookmarks %}
    <h3>📂 {{ group.category }}</h3>
    <ul>
      {% for bookmark in group.bookmarks %}
      <li>
        <a href="{{ bookmark.link }}" target="_blank">{{ bookmark.title }}</a>
      </li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>
