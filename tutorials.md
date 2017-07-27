---
layout: page
title: tuts

---

{% for poem in site.categories.formating %}
  {{poem.title}}
  
  {{ poem.output }}
  
{% endfor %}


[jekyll-organization]: https://github.com/jekyll
