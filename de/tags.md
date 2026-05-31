---
title: Schlagwörter
permalink: /de/tags/index.html
data:
  nav: posts
  lang: de
  alt_url: /tags/
---

# Nach Schlagwort stöbern

{%- capture tagblob -%}
{%- for post in collections.posts.pages -%}{%- if post.data.lang == "de" -%}{%- for t in post.tags -%}{{ t }}|{%- endfor -%}{%- endif -%}{%- endfor -%}
{%- endcapture -%}
{%- assign all_tags = tagblob | split: "|" | uniq | sort -%}

<div class="tag-cloud">
{%- for tag in all_tags -%}{%- if tag != "" -%}<a class="tag" href="#{{ tag }}">{{ tag }}</a>{%- endif -%}{%- endfor -%}
</div>

{% for tag in all_tags %}{% if tag != "" %}
<section class="tag-section" id="{{ tag }}">
<h2># {{ tag }}</h2>
<ul class="post-list">
{%- for post in collections.posts.pages -%}
  {%- if post.data.lang == "de" and post.tags contains tag -%}
  <li>
    <span class="date"><time datetime="{{ post.published_date | date: '%Y-%m-%d' }}">{{ post.published_date | date: "%d.%m.%Y" }}</time></span>
    <h3><a href="/{{ post.permalink }}">{{ post.title }}</a></h3>
    {%- if post.description -%}<p class="excerpt">{{ post.description }}</p>{%- endif -%}
  </li>
  {%- endif -%}
{%- endfor -%}
</ul>
</section>
{% endif %}{% endfor %}
