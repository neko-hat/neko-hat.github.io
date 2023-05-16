---
layout: default
title: posts
nav_order: 2
has_children: false
---

# POSTS

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>