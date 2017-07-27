---
title: Java
layout: page
---

{% for poem in site.categories.java %}
  {{poem.date}}
  <h3><a href="{{ post.url | relative_url }}">{{ poem.title }}</a></h3>
{% endfor %}
