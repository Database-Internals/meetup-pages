---
layout: hero
title: Meetups
---

> Список митапов сообщества Database Internals.

{% assign meetups = site.posts | where_exp:"post","post.tags contains 'meetup'" %}

{% if meetups and meetups != empty %}
{% include post-list.html posts=meetups %}
{% else %}
Пока нет митапов с тегом `meetup`.
{% endif %}

[Back to Home]({{ "/" | relative_url }})
