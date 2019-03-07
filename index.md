---
layout: default
---

{% for post in site.posts limit:25 %}

### {{ post.date | date_to_string: "ordinal", "ES" }}, [{{ post.title }}]({{ site.baseurl | append:post.url }})

<p>x de x de 2019: {{ post.date | date: "%e de %B de %Y" }}</p>

<p>00-00-2019: {{ post.date | date: "%d-%B-%Y" }}</p>

{{ post.excerpt }}

<a rel="full-article" href="{{ site.baseurl | append:post.url }}">
Continuar leyendo →
</a>

{% endfor %}

<hr>

Ver [todos los posts por categoría]({{ site.baseurl | append:"/all" }}), o ver
[todo, todo, todito]({{ site.baseurl | append:"/all-posts" }}).
