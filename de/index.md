---
title: Start
permalink: /de/index.html
data:
  nav: home
  lang: de
  alt_url: /
---

<section class="intro">
  <img class="portrait" src="/assets/img/alex.svg" alt="Porträt von Alexander Kaempf" width="168" height="168">
  <div class="intro-text">
    <!-- <p class="eyebrow">Mathematician</p> -->
    <h1>Alexander Kaempf</h1>
    <p>
      Ich bin Mathematik- und Physikstudent an der TUM und lebe derzeit in München. Mit diesem persönlichen Blog möchte ich einige allgemeine Gedanken zu Themen teilen, die mich interessieren – etwa Musikkomposition, Schreiben und Philosophie. Außerdem schreibe ich über technische Themen, hauptsächlich aus den Bereichen reine Mathematik, mathematische Physik, Quanteninformationstheorie und KI.
    </p>
    <p>
      Da ich noch nicht dazugekommen bin, die meisten Beiträge auf dieser Seite ins Deutsche zu übersetzen, empfehle ich, auf die englische Version zu wechseln. Dies können Sie machen, indem Sie den Button oben rechts betätigen.
    </p>
  </div>
</section>
<hr>
## Neueste Beiträge
<ul class="post-list">
{%- for post in collections.posts.pages -%}
  {%- if post.data.lang == "de" -%}
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
[Alle Beiträge →](/posts/)