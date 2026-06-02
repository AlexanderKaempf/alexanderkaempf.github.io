---
title: Home
permalink: /index.html
data:
  nav: home
  lang: en
  alt_url: /de/
---

<section class="intro">
  <img class="portrait" src="/assets/img/alex.svg" alt="Portrait of Alexander Kaempf" width="168" height="168">
  <div class="intro-text">
    <!-- <p class="eyebrow">Mathematician</p> -->
    <h1>Alexander Kaempf</h1>
    <p>
	I am a mathematics and physics student at TUM currently living in Munich, Germany. With this personal blog, I intend to share some general thoughts on topics which interest me, such as composing music, writing, and philosophy. I also write about technical topics, mainly in pure maths, mathematical physics, quantum information theory and AI.  
    </p>
  </div>
</section>

<hr>

## Recent writing

<ul class="post-list">
{%- for post in collections.posts.pages -%}
  {%- if post.data.lang == "en" -%}
  <li>
    <span class="date"><time datetime="{{ post.published_date | date: '%Y-%m-%d' }}">{{ post.published_date | date: "%B %-d, %Y" }}</time></span>
    <h3><a href="/{{ post.permalink }}">{{ post.title }}</a></h3>
    {%- if post.tags -%}
    <span class="tag-list">{%- for tag in post.tags -%}<a class="tag" href="/tags/#{{ tag }}">{{ tag }}</a>{%- endfor -%}</span>
    {%- endif -%}
  </li>
  {%- endif -%}
{%- endfor -%}
</ul>

[All posts →](/posts/)
