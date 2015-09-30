---
layout: archive
title2: "Coding Projects"
date: 2014-11-04T04:04:04-04:00
modified:
excerpt2: "If you optimize everything, you will always be unhappy."
tags: []
image:
  feature: coding.jpg
  teaser:
---

<div class="tiles">
{% for post in site.categories.coding %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->
