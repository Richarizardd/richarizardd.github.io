---
layout: blog_archive
title: "Publications and Research Projects"
date: 2014-06-02T15:05:16-04:00
modified: 2015-10-18T15:45:58-04:00
excerpt: "A selection of things I’ve designed, illustrated, and developed."
ads: false
fullwidth: true
feature:
  visible: false
  headline: "Featured Projects"
  category: research
---
{% for post in site.categories.research %}
  {% include archive__item_square.html %}
{% endfor %}
