---
title: Beiträge
permalink: /de/posts/index.html
data:
  nav: posts
  lang: de
  alt_url: /posts/
---

# Beiträge

Notizen und Essays über Zahlentheorie, Graphen und die Praxis der Mathematik.

<ul class="post-list">
{%- for post in collections.posts.pages -%}
  {%- if post.data.lang == "de" -%}
  <li>
    <span class="date"><time datetime="{{ post.published_date | date: '%Y-%m-%d' }}">{{ post.published_date | date: "%d.%m.%Y" }}</time></span>
    <h3><a href="/{{ post.permalink }}">{{ post.title }}</a></h3>
    {%- if post.description -%}<p class="excerpt">{{ post.description }}</p>{%- endif -%}
    {%- if post.tags -%}
    <span class="tag-list">{%- for tag in post.tags -%}<a class="tag" href="/de/tags/#{{ tag }}">{{ tag }}</a>{%- endfor -%}</span>
    {%- endif -%}
  </li>
  {%- endif -%}
{%- endfor -%}
</ul>
