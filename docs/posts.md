---
layout: default
title: posts
nav_order: 2
has_children: true
---

# POSTS

<ul>
  {% for post in site.pages %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>