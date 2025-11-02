---
layout: default
title: Home
---

<h1 class="page-index post-title">Database Internals Meetups</h1>

## Recent Videos:

  {% for post in site.posts %}
  {% if post.tags contains 'meetup' %}
  {% assign icon = "fab fa-meetup" %}
  {% else %}
  {% assign icon = "fas fa-podcast" %}
  {% endif %}
  <h1><i class="{{ icon }}"></i> <a href="{{ post.url | relative_url }}">{{ post.title }} — {{ post.date | date: "%Y-%m-%d" }}</a></h1>
  {% if post.excerpt %}
  {{ post.excerpt }}
  {% endif %}
{% endfor %}
