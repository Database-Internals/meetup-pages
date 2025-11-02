---
layout: post
title: Meetups
---

> Список митапов сообщества Database Internals.

{% assign meetups = site.posts | where_exp:"post","post.tags contains 'meetup'" %}

{% if meetups and meetups != empty %}
{% for post in meetups %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%Y-%m-%d" }}
  {% if post.excerpt %}
  
  {{ post.excerpt }}
  {% endif %}
{% endfor %}
{% else %}
Пока нет митапов с тегом `meetup`.
{% endif %}

[Back to Home]({{ "/" | relative_url }})
