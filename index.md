---
layout: default
title: "Blog de Informes DigiVision"
---

# 📑 Publicaciones

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> – {{ post.date | date: "%Y-%m-%d" }}
    </li>
  {% endfor %}
</ul>
