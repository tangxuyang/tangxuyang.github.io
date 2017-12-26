---
layout: default
title: pendding列表
---

{% for post in site.tags['static site'] %}
<a href="{{post.url}}">{{post.title}}</a>
{% endfor %}
