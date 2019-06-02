---
layout: page
title: Sitemap
permalink: /tags/
redirect_from: /tag/
sitemap: false
---

<!--Add a search bar on the sitemap page-->
<!-- <div id='search-box'>
  <form action='https://sh.yuanjiang.space/s' id='search-form' method='get' target='_blank_'>
      <input id='search-text' required name='q' placeholder='Enter text to search...' type='text'/>
      <button id='search-button' type='submit'>
          <span>Search</span>
      </button>
  </form>
  <div id="esPoweredBy"> -- powered by elasticsearch.</div>
</div>
<br/> -->

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
