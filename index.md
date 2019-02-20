---
layout: default
---

{% for post in site.posts limit:25 %}
### {{ forloop.index }}. [{{ post.title }}]({{ site.baseurl | append:post.url }})

{{ post.excerpt }}

{% endfor %}

<hr>

Ver [todos los posts por categoría]({{ site.baseurl | append:"/all" }}), o ver
[todo, todo, todito]({{ site.baseurl | append:"/all-posts" }}).
