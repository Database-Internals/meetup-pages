---
layout: default
title: Home
---

<img src="./assets/images/db_internals_meetup.jpg" alt="Banner Image" style="width:100%; height:auto;">

# Recent Videos

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>