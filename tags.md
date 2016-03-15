---
layout: page
title: Tags
permalink: /tags/
redirect_from: /tag/
sitemap: false
---

<div>
    {% assign tags = site.tags | sort %}
    {% for tag in tags %}
     <div class="site-tag">
        • <a href="/tag/{{ tag | first | slugify }}">
                {{ tag[0] | replace:'-', ' ' }}[{{ tag | last | size }}]
        </a>
    </div>
    {% endfor %}
</div>

<!-- <div id="index">
    {% for tag in tags %}
    <a name="{{ tag[0] }}"></a><h3>{{ tag[0] | replace:'-', ' ' }} ({{ tag | last | size }}) </h3>
    {% assign sorted_posts = site.posts | sort: 'title' %}
    {% for post in sorted_posts %}
    {%if post.tags contains tag[0]%}
      <h5><a href="{{ site.url }}{{site.baseurl}}{{ post.url }}" title="{{ post.title }}">{{ post.title }} </a></h5>
    {%endif%}
    {% endfor %}
    {% endfor %}
</div> -->
