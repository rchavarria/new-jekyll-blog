---
layout: default
---

{% for post in site.posts limit:25 %}
{% include article.html %}
{% endfor %}

<hr>

Ver [todos los posts por categoría]({{ site.baseurl | append:"/all" }}), o ver
[todo, todo, todito]({{ site.baseurl | append:"/all-posts" }}).
