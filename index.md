---
layout: page
title: WXYZG
tagline: Simply a Blog
---
{% include JB/setup %}

I am a blog for technical stuff which the <a href="http://wei.wxyzg.com">author</a> found interesting.

## Posts List

Here's a posts list.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



