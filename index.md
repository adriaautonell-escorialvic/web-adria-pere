---
layout: default
title: Inici
---

<div class="nc-hero" markdown="1">

# NetCore Systems

Benvinguts al portal de documentació del **departament de sistemes** de NetCore Systems.

NetCore Systems és una empresa *fictícia* que ens hem inventat per a aquesta pràctica: una petita empresa que munta i manté xarxes i servidors Linux per a altres empreses.

</div>

## Per què existeix aquesta web

Quan algú entra nou al departament sempre pregunta les mateixes coses: com es crea un usuari, quins ports fa servir SSH, per què no resol un domini... La idea és tenir-ho tot **escrit en un sol lloc** i que qualsevol administrador ho pugui consultar ràpid.

Els objectius de la web són:

1. Tenir guies curtes i pràctiques, amb les comandes que fem servir cada dia.
2. Que la informació estigui ordenada per temes.
3. Que qualsevol del departament pugui afegir o corregir una pàgina fàcilment (tot és Markdown).

## Seccions

<div class="nc-cards">
  <div class="nc-card">
    <h3><a href="{{ '/linux/' | relative_url }}">🐧 Linux</a></h3>
    <p>Gestió d'usuaris i grups, i permisos de fitxers.</p>
  </div>
  <div class="nc-card">
    <h3><a href="{{ '/xarxes/' | relative_url }}">🌐 Xarxes</a></h3>
    <p>Model TCP/IP, adreces IP i com funciona el DNS.</p>
  </div>
  <div class="nc-card">
    <h3><a href="{{ '/serveis/' | relative_url }}">🖥️ Serveis</a></h3>
    <p>Accés remot amb SSH i servidors web HTTP.</p>
  </div>
  <div class="nc-card">
    <h3><a href="{{ '/blog/' | relative_url }}">📝 Blog</a></h3>
    <p>Articles més llargs amb casos pràctics.</p>
  </div>
</div>

## Últims articles

<ul class="nc-posts">
{%- for post in site.posts limit: 3 %}
  <li>
    <p class="nc-meta">{{ post.date | date: "%d/%m/%Y" }} · {{ post.author }}</p>
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
  </li>
{%- endfor %}
</ul>

> **Nota:** tota la informació d'aquesta web està pensada per a Debian/Ubuntu, que és el que fem servir a l'institut. En altres distribucions alguna comanda pot canviar.
{: .consell }
