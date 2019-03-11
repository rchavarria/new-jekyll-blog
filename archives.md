---
layout: default
title: "Archivo"
---

{% for post in site.posts reverse %}

<article>
  <div>
    {{ post.date }}
  </div>
  <h1>
    <a href="{{ site.baseurl | append:post.url }}">
      {{ post.title }}
    </a>
  </h1>
  <div>
    publicado en: {{ post.categories }}
  </div>
</article>

{% endfor %}
