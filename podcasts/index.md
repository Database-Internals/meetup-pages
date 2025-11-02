---
layout: post
title: Podcasts
---


<script>
// Add a page class so fixup.css can target the .post-title only on this page
document.addEventListener('DOMContentLoaded', function () {
  if (document && document.body) document.body.classList.add('page-podcasts');
});
</script>

> Список подкастов сообщества Database Internals.

{% assign podcasts = site.posts | where_exp:"post","post.tags contains 'podcast'" %}

{% if podcasts and podcasts != empty %}
{% for post in podcasts %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%Y-%m-%d" }}
  {% if post.excerpt %}
  
  {{ post.excerpt }}
  {% endif %}
{% endfor %}
{% else %}
Пока нет подкастов с тегом `podcast`.
{% endif %}

[Back to Home]({{ "/" | relative_url }})
