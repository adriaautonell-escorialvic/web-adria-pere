---
title: Blog
permalink: /blog/
---

Aquí anem penjant articles més llargs que les pàgines de documentació. Normalment són casos pràctics: com hem fet alguna cosa pas a pas, amb les comandes i el que hem après.

<ul class="nc-posts">
{%- for post in site.posts %}
  <li>
    <p class="nc-meta">{{ post.date | date: "%d/%m/%Y" }} · {{ post.author }}</p>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    <p>{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
  </li>
{%- endfor %}
</ul>

Si fas servir un lector de notícies, pots seguir el blog amb el [RSS]({{ '/feed.xml' | relative_url }}).
