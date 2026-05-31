---
title: Start
permalink: /de/index.html
data:
  nav: home
  lang: de
  alt_url: /
---

<section class="intro">
  <img class="portrait" src="/assets/img/portrait.svg" alt="Porträt von Dr. Elena Voss" width="168" height="168">
  <div class="intro-text">
    <p class="eyebrow">Mathematikerin</p>
    <h1>Dr. Elena Voss</h1>
    <p>
      Ich bin Mathematikerin und arbeite in der analytischen Zahlentheorie und
      der Spektralgraphentheorie. Meine Forschung untersucht die Verteilung der
      Primzahlen, die Eigenwert-Geometrie großer Netzwerke und die stillen
      Brücken zwischen beiden. Auf dieser Seite sammle ich meine Notizen, Essays
      und gelegentliche Gedanken zur Lehre und zum Handwerk der Mathematik.
    </p>
  </div>
</section>

<hr>

## Neueste Beiträge

<ul class="post-list">
{%- for post in collections.posts.pages -%}
  {%- if post.data.lang == "de" -%}
  <li>
    <span class="date"><time datetime="{{ post.published_date | date: '%Y-%m-%d' }}">{{ post.published_date | date: "%d.%m.%Y" }}</time></span>
    <h3><a href="/{{ post.permalink }}">{{ post.title }}</a></h3>
    {%- if post.tags -%}
    <span class="tag-list">{%- for tag in post.tags -%}<a class="tag" href="/de/tags/#{{ tag }}">{{ tag }}</a>{%- endfor -%}</span>
    {%- endif -%}
  </li>
  {%- endif -%}
{%- endfor -%}
</ul>

[Alle Beiträge →](/de/posts/)
