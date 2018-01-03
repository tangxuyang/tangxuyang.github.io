---
layout: default
title: 读书笔记列表
---

{% for post in site.tags['reading note'] %}
<a href="{{ post.url }}">{{post.title}}</a>
{% endfor %}