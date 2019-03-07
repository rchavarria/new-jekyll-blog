---
layout: default
---

{% for post in site.posts limit:25 %}

### {{ post.date }}, [{{ post.title }}]({{ site.baseurl | append:post.url }})

{{ post.excerpt }}

<a rel="full-article" href="{{ site.baseurl | append:post.url }}">
Continuar leyendo →
</a>

{% endfor %}

<hr>

Ver [todos los posts por categoría]({{ site.baseurl | append:"/all" }}), o ver
[todo, todo, todito]({{ site.baseurl | append:"/all-posts" }}).
