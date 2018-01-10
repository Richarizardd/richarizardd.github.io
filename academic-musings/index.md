---
layout: archive
title: "Academic musings of an inspiring researcher"
excerpt: "Reading one research paper every day."
share: false
image: 
  feature: academic-musings-feature.jpg
feature:
  visible: true
  headline: "Recent Articles"
  category: academic-musings
---

{% for post in site.categories.academic-musings %}
  {% include archive__item.html %}
{% endfor %}
