---
title: Lifehacks with CS
layout: page
---

{% for poem in site.categories.formating %}
  {{poem.date}}
  <h3><a href="{{ poem.url }}">{{ poem.title }}</a></h3>
{% endfor %}

