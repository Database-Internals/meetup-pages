---
layout: default
title: About Us
---

<div class="gallery">
  {% for item in site.data.people %}
  <div class="gallery-item">
    <img src="{{ item.image | relative_url }}" alt="{{ item.name }}" />
    <h3>{{ item.name }}</h3>
    <p class="grey">{{ item.description }}</p>
  </div>
  {% endfor %}
</div>

<style>
.gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
.gallery-item {
  flex: 1 1 calc(33.333% - 16px);
  box-sizing: border-box;
  text-align: center;
}
.gallery-item img {
  max-width: 100%;
  max-height: 238px;
  height: auto;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-shadow: 0 8px 8px rgba(0, 0, 0, 0.3);
}
</style>