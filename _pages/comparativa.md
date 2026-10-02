---
title: "Comparativa: Jekyll, Hugo i MkDocs"
permalink: /comparativa/
---

Abans de fer la web vam mirar els tres generadors de webs estàtiques que proposava la pràctica. Aquí hi ha el resum del que vam trobar a la documentació oficial de cadascun.

## Què tenen en comú

Tots tres fan el mateix en el fons: agafen fitxers **Markdown** + una **configuració** + un **tema**, i generen una carpeta amb **HTML, CSS i JS** que es pot penjar a qualsevol servidor web (o a GitHub Pages).

## Taula comparativa

| | Jekyll | Hugo | MkDocs |
|---|---|---|---|
| **Llenguatge** | Ruby | Go | Python |
| **Instal·lació** | Ruby + `gem install jekyll bundler` | Un sol executable | `pip install mkdocs` |
| **Fitxer de configuració** | `_config.yml` | `hugo.toml` / `hugo.yaml` | `mkdocs.yml` |
| **Plantilles** | Liquid | Go templates | Jinja2 (dins del tema) |
| **Facilitat d'ús** | Fàcil | Mitjana (les plantilles costen més) | Molt fàcil |
| **Velocitat** | Correcta; va més lent amb webs molt grans | Molt ràpid, fins i tot amb milers de pàgines | Ràpid per a documentació |
| **Temes** | Molts, sobretot per a blogs | Molts i molt variats | Pocs, però *Material for MkDocs* és molt complet |
| **Markdown** | kramdown (per defecte) | Goldmark | Python-Markdown + extensions |
| **Blogs** | Molt bo, els `_posts` venen de sèrie | Molt bo | No és el seu punt fort (cal plugin o tema) |
| **Documentació tècnica** | Bé, però cal muntar-se la navegació | Molt bé | Excel·lent, està fet per això (cercador inclòs) |
| **GitHub Pages** | Integració directa (GitHub el pot generar sense configurar res) | Cal GitHub Actions | `mkdocs gh-deploy` o GitHub Actions |
| **Servidor local** | `jekyll serve` | `hugo server` | `mkdocs serve` |

## Avantatges i inconvenients

### Jekyll

- ✅ És el que GitHub Pages fa servir per defecte, així que hi ha molta documentació sobre com publicar-lo.
- ✅ Pensat per a blogs: dates, autors, RSS... tot és molt directe.
- ✅ Liquid és fàcil d'entendre.
- ❌ Instal·lar Ruby pot donar problemes (a macOS, per exemple, la Ruby del sistema és massa antiga).
- ❌ Amb webs molt grans, la generació és més lenta que la de Hugo.

### Hugo

- ✅ Molt ràpid.
- ✅ Només cal descarregar un executable, sense dependències.
- ✅ Té idiomes, taxonomies i seccions sense plugins.
- ❌ Les plantilles de Go són més difícils d'aprendre.
- ❌ Hi ha molts conceptes (*sections*, *bundles*, *archetypes*...) i al principi costa.

### MkDocs

- ✅ El més fàcil per a documentació: escrius Markdown, poses el menú a `mkdocs.yml` i ja està.
- ✅ Porta cercador integrat.
- ✅ Amb el tema Material queda molt bé sense tocar CSS.
- ❌ Està pensat gairebé només per a documentació. Per a un blog o una web corporativa no és el més adequat.
- ❌ Es pot personalitzar menys que els altres dos.

## Per què hem fet servir Jekyll

La pràctica ens assignava Jekyll, però a més a més ens ha semblat una bona opció per aquest projecte:

- La web té una part de **blog** (els articles) i una part de **documentació**, i Jekyll fa bé les dues coses.
- Està molt lligat a **GitHub Pages**, que és on publiquem.
- L'estructura de carpetes (`_posts`, `_layouts`, `_includes`...) és fàcil d'explicar.

Ara bé, si només haguéssim de fer documentació tècnica amb centenars de pàgines, segurament triaríem **MkDocs** (o **Hugo** si fos una web molt gran). No hi ha un generador millor que els altres per a tot. Depèn del projecte.

## Fonts consultades

- Documentació de Jekyll: <https://jekyllrb.com/docs/>
- Documentació de Hugo: <https://gohugo.io/documentation/>
- Documentació de MkDocs: <https://www.mkdocs.org/>
- GitHub Pages i Jekyll: <https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll>
