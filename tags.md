---
layout: page
title: Sitemap
permalink: /tags/
redirect_from: /tag/
sitemap: false
---

<!--Add a search bar on the sitemap page-->
<div id="esSearch">
  <form action="http://sh.yuanjiang.space/s" method="get" target="_blank">
    <!-- <input type="submit" value="GO" id="esSearchButton"></input> -->
    <div id="esSearchBar">
      <input type="text" name="q" id="esSearchString" placeholder="Enter text to search..." />
     </div>
     <div id="esPoweredBy"> -- powered by elasticsearch.</div>
  </form>
</div>

<br/>

<div style="word-break:break-all;">
    {% assign tags = site.tags | sort %}
    {% for tag in tags %}
     <span class="site-tag">
        • <a href="/tag/{{ tag | first | slugify }}" target="_blank">
                {{ tag[0] | replace:'-', ' ' }}<sup>{{ tag | last | size }}</sup>
        </a>
    </span>
    {% endfor %}
</div>

<!-- <div>
    {% assign tags = site.tags | sort %}
    {% for tag in tags %}
     <div class="site-tag">
        • <a href="/tag/{{ tag | first | slugify }}">
                {{ tag[0] | replace:'-', ' ' }}[{{ tag | last | size }}]
        </a>
    </div>
    {% endfor %}
</div> -->

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
