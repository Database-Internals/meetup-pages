---
layout: post
title: Meetups
---


<script>
// Add a page class so fixup.css can target the .post-title only on this page
document.addEventListener('DOMContentLoaded', function () {
  if (document && document.body) document.body.classList.add('page-meetups');
});
</script>

> Список митапов сообщества Database Internals.

{% assign meetups = site.posts | where_exp:"post","post.tags contains 'meetup'" %}

{% if meetups and meetups != empty %}
{% for post in meetups %}
  <h1><i class="fab fa-meetup"></i> <a href="{{ post.url | relative_url }}">{{ post.title }} — {{ post.date | date: "%Y-%m-%d" }}</a></h1>

  {% if post.excerpt %}
  {{ post.excerpt }}
  {% endif %}
{% endfor %}
{% else %}
Пока нет митапов с тегом `meetup`.
{% endif %}

[Back to Home]({{ "/" | relative_url }})
