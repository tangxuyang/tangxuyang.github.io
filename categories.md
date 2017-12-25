---
title: 分类列表
layout: default
---

{% for category in site.categories %}
	<a href="/category/{{category | first}}.html">{{category | fist}} ({{category | last | size}})</a>
{% endfor %}