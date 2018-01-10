---
layout: blog_archive
title: "Blog Articles"
date: 2014-06-02T12:26:34-04:00
modified: 2014-08-18T14:21:32-04:00
excerpt: "A collection of thoughts, inspiration, mistakes, and other minutia I’ve written."
share: false
fullwidth: true
tiles: false
image: 
  feature1: blog/feature1.jpg
  feature2: blog/feature2.jpg
  feature3: blog/feature3.jpg
  feature4: blog/feature4.jpg
feature:
  visible: false
  headline: "Recent blogs"
  category: blog
---

{% for post in site.categories.blog %}
  {% include archive__item.html %}
{% endfor %}
