---
layout: page
title: andrewreitz.com
tagline: Reitz; pronounced rights
---
{% include JB/setup %}

Hi! I'm Andrew, a Developer in Minneapolis, M.N.  I currently am a Android Developer at [The Nerdery](http://nerdery.com/).

## Blog Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	<p>{{ post.excerpt }} </p>
  {% endfor %}
</ul>
