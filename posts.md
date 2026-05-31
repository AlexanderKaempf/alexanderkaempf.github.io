---
title: Posts
permalink: /posts/index.html
data:
  nav: posts
  lang: en
  alt_url: /de/posts/
---

# Posts

Notes and essays on number theory, graphs, and the practice of mathematics.

<ul class="post-list">
{%- for post in collections.posts.pages -%}
  {%- if post.data.lang == "en" -%}
  <li>
    <span class="date"><time datetime="{{ post.published_date | date: '%Y-%m-%d' }}">{{ post.published_date | date: "%B %-d, %Y" }}</time></span>
    <h3><a href="/{{ post.permalink }}">{{ post.title }}</a></h3>
    {%- if post.description -%}<p class="excerpt">{{ post.description }}</p>{%- endif -%}
    {%- if post.tags -%}
    <span class="tag-list">{%- for tag in post.tags -%}<a class="tag" href="/tags/#{{ tag }}">{{ tag }}</a>{%- endfor -%}</span>
    {%- endif -%}
  </li>
  {%- endif -%}
{%- endfor -%}
</ul>
