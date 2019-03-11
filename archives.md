---
layout: default
title: "Archivo"
---

{% for post in site.posts reverse %}

<article>
  <header>
    {{ post.date }}
  </header>
  <content>
    <h1>
      <a href="{{ site.baseurl | append:post.url }}">
        {{ post.title }}
      </a>
    </h1>
  </content>
  <footer>
    publicado en: {{ post.categories }}
  </footer>
</article>

{% endfor %}
