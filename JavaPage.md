---
title: Datastructures
layout: page
---

{% for poem in site.categories.datastructures %}
  {{poem.date}}
  <h3><a href="{{ poem.url | relative_url }}">{{ poem.title }}</a></h3>
{% endfor %}
