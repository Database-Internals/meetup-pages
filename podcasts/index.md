---
layout: hero
title: Podcasts
---

> Список подкастов сообщества Database Internals.

{% assign podcasts = site.posts | where_exp:"post","post.tags contains 'podcast'" %}

{% if podcasts and podcasts != empty %}
{% include post-list.html posts=podcasts %}
{% else %}
Пока нет подкастов с тегом `podcast`.
{% endif %}

[Back to Home]({{ "/" | relative_url }})
