---
layout: default
title: About Us
---

<div class="gallery">
  {% for item in site.data.people %}
  <figure class="gallery-item">
    <img src="{{ item.image | relative_url }}" alt="{{ item.name }}" loading="lazy" />
    <figcaption>
      <h3>{{ item.name }}</h3>
      {% if item.description %}<p class="grey">{{ item.description }}</p>{% endif %}
    </figcaption>
  </figure>
  {% endfor %}
</div>
