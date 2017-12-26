---
layout: default
title: tag索引页
---

{% for tag in site.tags %}
<a href="/tag/{{tag | first}}.html">{{tag | first}}({{tag | last | size}})</a>
{% endfor %}