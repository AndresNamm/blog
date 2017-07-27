---
layout: page
title: ML and AI

---


# About 

Mostly the tutorials here are summaries with added value of these 2 courses:

1. [Andrew NG coursera Machine Learning ](https://www.coursera.org/learn/machine-learning) Source code available in my github repo.
2. [Berkeley AI course](http://ai.berkeley.edu/home.html ) Source code available in my github repo. 


# Posts

{% for poem in site.categories.machine-learning %}
  {{poem.date}}
  <h3><a href="{{ poem.url | relative_url }}">{{ poem.title }}</a></h3>
{% endfor %}


[jekyll-organization]: https://github.com/jekyll
