---
layout: default
title: "Todos los artículos"
---

{% for post in site.posts %}
- {{ post.date }}: [{{ post.title }}]({{ site.baseurl | append:post.url }})
{% endfor %}
