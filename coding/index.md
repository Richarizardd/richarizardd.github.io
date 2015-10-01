---
layout: archive
title2: "Coding Projects"
excerpt2: "If you optimize everything, you will always be unhappy."
date: 2014-11-04T04:04:04-04:00
modified:
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
