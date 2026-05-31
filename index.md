---
title: Home
permalink: /index.html
data:
  nav: home
  lang: en
  alt_url: /de/
---

<section class="intro">
  <img class="portrait" src="/assets/img/portrait.svg" alt="Portrait of Dr. Alexander Kaempf" width="168" height="168">
  <div class="intro-text">
    <p class="eyebrow">Mathematician</p>
    <h1>Dr. Alexander Kaempf</h1>
    <p>
      I am a cool mathematician working in analytic number theory and spectral graph
      theory. My research explores the distribution of prime numbers, the
      eigenvalue geometry of large networks, and the quiet bridges between the
      two. This site collects my notes, essays, and occasional thoughts on
      teaching and the craft of doing mathematics.
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
