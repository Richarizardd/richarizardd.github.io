---
layout: archive
title2: "Research Projects"
excerpt2: "Furious activity is no substitute for understanding."
date: 2015-03-001T09:44:20-04:00
modified:
image:
  feature: projects.jpg
  teaser:
---

<div class="tiles">
{% for post in site.categories.research %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->