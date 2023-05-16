---
layout: default
title: POSTS
nav_order: 2
has_children: false
post_viewer: true
---

# POSTS

<ul>
  {% for post in site.pages %}
  {% if post.hidden != true %}
  {% if post.post_viewer != true %}
  {% if post.nav_exclude != true %}
  {% if post.hs_child != true %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endif %}
    {% endif %}
    {% endif %}
    {% endif %}
  {% endfor %}
</ul>
