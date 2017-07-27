---
layout: page
title: ML and AI

---

{% for poem in site.categories.machine-learning %}
  {{poem.date}}
  <h3><a href="{{ site.baseurl }}{{ poem.url }}">{{ poem.title }}</a></h3>
{% endfor %}


[jekyll-organization]: https://github.com/jekyll
