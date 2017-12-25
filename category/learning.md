---
layout: default
---

{% for post in site.categories['learning'] %}
	<a href="{{post.url}}">{{post.title}}</a>
{% endfor %}