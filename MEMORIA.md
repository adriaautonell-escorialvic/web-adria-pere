# Memòria: Creació i publicació d'una web estàtica amb GitHub Pages

**Mòdul:** Implantació d'aplicacions web, ASIX 2n curs
**Alumnes:** Adrià Autonell i Pere [OMPLIR cognom]
**Eina assignada:** Jekyll
**Data d'entrega:** [OMPLIR]
**Repositori:** [POSAR URL DEL REPOSITORI]
**Web publicada:** [POSAR URL DE GITHUB PAGES]

> **Llegenda de l'estat** (per saber què està fet i què falta):
> - ✅ **Configurat:** està fet al projecte.
> - 🔍 **Comprovat:** ho hem provat i funciona. Posar-ho només quan ho hàgim vist nosaltres.
> - ⏳ **Pendent de comprovar**

---

## Índex

1. Introducció
2. Objectius
3. Què és un SSG
4. Per què hem utilitzat Jekyll
5. Comparativa Hugo / Jekyll / MkDocs
6. Instal·lació
7. Creació del projecte
8. Estructura del projecte
9. Configuració
10. Creació de contingut
11. Markdown
12. Personalització
13. Git
14. GitHub
15. GitHub Pages
16. GitHub Actions
17. Generació de la web
18. Problemes trobats
19. Solucions
20. Proves
21. Resultat final
22. Conclusions
- Annex A: Qüestionari final
- Annex B: Repte final
- Annex C: Estat del projecte

---

## 1. Introducció

En aquesta pràctica hem creat una web estàtica de documentació tècnica per a una empresa inventada, **NetCore Systems**, que es dedica a muntar i mantenir xarxes i servidors Linux. La web està pensada perquè un administrador de sistemes hi pugui consultar coses ràpidament: com crear usuaris, com funcionen els permisos, el DNS, SSH, etc.

L'hem feta amb **Jekyll**, un generador de webs estàtiques. El codi el tenim a **GitHub** i la web es publica amb **GitHub Pages**. A més, hem automatitzat el desplegament amb **GitHub Actions**, perquè cada vegada que fem `git push` la web es torni a generar i publicar sola.

## 2. Objectius

- Entendre què és una web estàtica i què és un Static Site Generator.
- Escriure contingut en Markdown.
- Treballar amb Git i GitHub des del terminal, entre dues persones i amb branques.
- Configurar i personalitzar una web amb Jekyll.
- Publicar-la a GitHub Pages i automatitzar-ho amb GitHub Actions.
- Comparar Jekyll amb Hugo i MkDocs.

## 3. Què és un SSG

Un **Static Site Generator** és un programa que agafa fitxers de contingut (normalment Markdown), una configuració i unes plantilles, i genera una web feta només de fitxers **HTML, CSS i JS**.

```text
Markdown + configuració + tema
              ↓
     Static Site Generator
              ↓
      HTML + CSS + JS
```

La diferència amb un WordPress, per exemple, és que aquí **no hi ha base de dades ni PHP**. Les pàgines es generen una sola vegada, quan fem el *build*, i el servidor només ha d'entregar fitxers.

## 4. Per què hem utilitzat Jekyll

Jekyll era l'eina que ens van assignar. Tot i així, creiem que és una bona tria per a aquest projecte:

- La web té una part de **blog**, i Jekyll està pensat per a això (carpeta `_posts`, dates, autors, RSS).
- GitHub Pages està molt lligat a Jekyll, i hi ha molta documentació oficial sobre com fer-los servir junts.
- Les plantilles (Liquid) són senzilles d'entendre.

## 5. Comparativa Hugo / Jekyll / MkDocs

| | Jekyll | Hugo | MkDocs |
|---|---|---|---|
| **Llenguatge / entorn** | Ruby (gems, Bundler) | Go (un sol executable) | Python (pip) |
| **Facilitat d'ús** | Fàcil | Mitjana | Molt fàcil |
| **Velocitat** | Correcta; més lent en webs grans | Molt ràpid | Ràpid |
| **Documentació oficial** | Bona, amb molts tutorials | Molt completa però densa | Curta i clara |
| **Temes** | Molts, sobretot de blogs | Molts i variats | Pocs, però Material és molt complet |
| **Markdown** | kramdown | Goldmark | Python-Markdown |
| **Blogs** | Molt bo (de sèrie) | Molt bo | Fluix (cal plugin) |
| **Documentació tècnica** | Correcte; la navegació cal fer-la | Molt bo | Excel·lent (cercador inclòs) |
| **GitHub Pages** | Integració directa | Amb GitHub Actions | `mkdocs gh-deploy` o Actions |
| **Configuració** | `_config.yml` | `hugo.toml` | `mkdocs.yml` |

**Avantatges i inconvenients:**

- **Jekyll.** ✅ Fàcil, pensat per a blogs, integrat amb GitHub Pages. ❌ Instal·lar Ruby pot donar problemes, i és més lent amb webs molt grans.
- **Hugo.** ✅ Molt ràpid, sense dependències, té suport per a diversos idiomes. ❌ Les plantilles de Go i els conceptes (*sections*, *bundles*...) costen més d'aprendre.
- **MkDocs.** ✅ El més ràpid per començar a fer documentació, amb cercador inclòs. ❌ Poc flexible per a coses que no siguin documentació.

**Conclusió:** per aquesta pràctica Jekyll és adequat perquè barreja blog i documentació i es publica fàcilment a GitHub Pages. No vol dir que sigui el millor per a tot: per a una documentació molt gran triaríem MkDocs o Hugo (vegeu l'annex B).

## 6. Instal·lació

**Situació inicial (ordinador de l'Adrià, macOS):** Jekyll no estava instal·lat, i la Ruby del sistema era la `2.6.10`, massa antiga per a les versions actuals de Jekyll. La documentació oficial de Jekyll recomana no fer servir la Ruby de macOS.

Passos seguits (guia oficial de Jekyll per a macOS):

```bash
brew install chruby ruby-install
ruby-install ruby 3.3
# + configuració de chruby a ~/.zshrc (vegeu el README)
gem install bundler
```

| Dada | Adrià | Pere |
|---|---|---|
| Sistema operatiu | macOS [OMPLIR versió] | [OMPLIR] |
| `ruby -v` | [OMPLIR] | [OMPLIR] |
| `bundle -v` | [OMPLIR] | [OMPLIR] |
| `bundle exec jekyll -v` | [OMPLIR] | [OMPLIR] |

[CAPTURA DE PANTALLA: `ruby -v` i `jekyll -v`]

## 7. Creació del projecte

Jekyll té la comanda `jekyll new` per crear un projecte d'exemple. Nosaltres hem creat l'estructura a mà, a partir de la documentació, perquè volíem entendre cada fitxer i no tenir coses que no féssim servir. El punt de partida són només dos fitxers: el `Gemfile` (quines gems cal instal·lar) i el `_config.yml` (configuració).

```bash
bundle install
bundle exec jekyll serve
```

## 8. Estructura del projecte

| Fitxer / carpeta | Per a què serveix |
|---|---|
| `_config.yml` | Configuració general: títol, descripció, autor, URL, tema, plugins |
| `Gemfile` / `Gemfile.lock` | Gems que necessita el projecte i les seves versions exactes |
| `index.md` | Pàgina d'inici |
| `_pages/` | Pàgines de documentació, blog, comparativa i qui som |
| `_posts/` | Articles del blog. El nom ha de ser `AAAA-MM-DD-titol.md` |
| `_layouts/` | Plantilles. Hem creat `doc.html` per a les pàgines de documentació |
| `_includes/` | Trossos d'HTML reutilitzables (capçalera, peu, menú lateral) |
| `_data/navigation.yml` | Les opcions del menú principal |
| `assets/css/` | El nostre CSS |
| `assets/images/` | Logo, favicon i diagrames |
| `.github/workflows/` | Workflow de GitHub Actions |
| `_site/` | Web generada. **No es puja a Git** |

## 9. Configuració

El fitxer principal és `_config.yml`. Les decisions que hem pres:

- `title`, `description`, `author` i `lang: ca`, que surten a la web i a les etiquetes `<meta>`.
- `theme: minima`: el tema per defecte de Jekyll. És senzill i així es nota més el que hem personalitzat nosaltres.
- `url` i `baseurl`: en local es deixen buits. Quan es publica, el workflow passa el `baseurl` (`/nom-del-repositori`) amb l'opció `--baseurl`. Així no hem de canviar res entre local i GitHub.
- `include: [_pages]`: Jekyll ignora les carpetes que comencen per `_`, i així li diem que processi `_pages`.
- `permalink: /blog/:year/:month/:day/:title/`: format de l'URL dels articles.
- `defaults`: els posts fan servir el layout `post` i les pàgines el layout `page` sense haver-ho de posar a cada fitxer.
- `plugins`: `jekyll-feed` (RSS), `jekyll-seo-tag` (SEO) i `jekyll-sitemap` (sitemap).
- `exclude`: fitxers que no han d'aparèixer a la web (README, MEMORIA, Gemfile...).
- `timezone: Europe/Madrid` perquè les dates dels articles surtin amb la nostra hora.

El **menú** es defineix a `_data/navigation.yml` i el llegeix `_includes/header.html`. Per afegir una opció al menú només cal afegir-hi dues línies.

## 10. Creació de contingut

| Secció | Pàgines | Autor |
|---|---|---|
| Linux | Portada, Usuaris i grups, Permisos | Adrià |
| Xarxes | Portada, TCP/IP, DNS | Pere |
| Serveis | Portada, SSH, HTTP | Pere |
| Blog | 1 article (usuaris i permisos) | Adrià |
| Blog | 2 articles (DNS, SSH) | Pere |
| Altres | Inici, Blog, Comparativa, Qui som | Tots dos |

Cada pàgina comença amb un **front matter** (el bloc entre `---`) on posem el títol, la URL, el layout, la secció i l'ordre dins del menú lateral:

```yaml
---
title: Permisos
permalink: /linux/permisos/
layout: doc
section: linux
order: 2
author: Adrià Autonell
---
```

Els **articles** porten el títol, la data i l'autor, i tenen almenys una imatge i exemples de comandes.

## 11. Markdown

Hem fet servir tots els elements que demanava la pràctica:

| Element | Exemple on es fa servir |
|---|---|
| Títols i subtítols (`#`, `##`, `###`) | Totes les pàgines |
| Llistes numerades i de punts | Inici, Xarxes, articles |
| **Negreta** i *cursiva* | Totes les pàgines |
| Enllaços | Portades de secció, comparativa |
| Imatges | Permisos, TCP/IP, DNS, SSH, HTTP i els 3 articles |
| Taules | Permisos, TCP/IP, DNS, HTTP... |
| Blocs de codi amb color (`bash`) | Totes les pàgines tècniques |

També hem fet servir una funció de **kramdown** per posar una classe CSS a una cita (`{: .avis }`) i fer caixes d'avís de colors.

## 12. Personalització

- **Nom, descripció i menú propis** (`_config.yml`, `_data/navigation.yml`).
- **CSS propi** (`assets/css/netcore.css`): colors de l'empresa, capçalera, targetes de la pàgina d'inici, taules, blocs de codi, caixes d'avís i versió per a mòbil.
- **Logo i favicon** en SVG fets per nosaltres.
- **Plantilles sobreescrites** del tema minima: `head.html` (favicon i CSS), `header.html` (logo i menú) i `footer.html`.
- **Layout propi `doc.html`** amb menú lateral automàtic (`section-nav.html`) i ruta (*Inici › Secció › Pàgina*).
- **Menú per a mòbil** fet només amb CSS (un checkbox amagat).
- **Diagrames SVG propis** per a cada tema. Els hem fet nosaltres, així que no hi ha problemes de llicència.
- **Extres:** RSS (`/feed.xml`), sitemap (`/sitemap.xml`), `robots.txt` i pàgina 404.

Per sobreescriure una part del tema, només cal crear un fitxer amb el **mateix nom** a la nostra carpeta `_includes` o `_layouts`. Jekyll fa servir el nostre en lloc del del tema.

## 13. Git

Git és el sistema de control de versions que hem fet servir. Comandes utilitzades:

| Comanda | Per a què |
|---|---|
| `git init` / `git clone` | Crear el repositori / descarregar-lo |
| `git status` | Veure quins fitxers han canviat |
| `git add` | Preparar els canvis per al commit |
| `git commit -m` | Guardar els canvis amb un missatge |
| `git push` | Enviar els commits a GitHub |
| `git pull` | Baixar els canvis de GitHub |
| `git switch -c` | Crear una branca i canviar-hi |
| `git merge` | Unir una branca amb una altra |
| `git log --oneline --graph` | Veure l'historial |

**Organització amb branques:**

```text
main  ──●──●──●─────────────●────────●──●
             │ \           /        /
             │  adria ──●──●──●    /
             │                    /
             └─ pere ──●──●──●──●
```

- `main`: la branca principal. És la que es publica.
- `adria`: configuració de Linux, primer article, GitHub Actions.
- `pere`: Xarxes, Serveis, articles de DNS i SSH.
- Quan cada branca estava acabada, s'ha fet `merge` a `main`.

[CAPTURA DE PANTALLA: `git log --oneline --graph --all`]

## 14. GitHub

- Repositori: [POSAR URL DEL REPOSITORI]
- Creat per: [OMPLIR usuari de GitHub]
- L'altre membre s'ha afegit com a col·laborador (*Settings → Collaborators*).
- Branques pujades: `main`, `adria`, `pere`.

[CAPTURA DE PANTALLA: repositori a GitHub]

## 15. GitHub Pages

**GitHub Pages** és el servei de GitHub que publica una web estàtica a partir d'un repositori.

Hi ha dues maneres de configurar-lo:

1. **Deploy from a branch:** GitHub genera la web ell mateix, però amb la seva versió de Jekyll i només amb uns plugins concrets.
2. **GitHub Actions:** som nosaltres qui diem com es genera la web. És la que hem fet servir, perquè així GitHub fa servir la mateixa versió de Jekyll que tenim al `Gemfile`.

Configuració: *Settings → Pages → Build and deployment → Source: GitHub Actions*.

URL final: [POSAR URL DE GITHUB PAGES]

[CAPTURA DE PANTALLA: Settings → Pages]

## 16. GitHub Actions

**GitHub Actions** és el sistema d'automatització de GitHub. Un **workflow** és un fitxer YAML a `.github/workflows/` que diu *quan* s'ha de fer una tasca i *quins passos* té.

El nostre workflow (`.github/workflows/jekyll.yml`):

- **Quan s'executa:** a cada `push` a `main`, o a mà (`workflow_dispatch`).
- **Feina `build`** (en una màquina virtual Ubuntu de GitHub):
  1. `actions/checkout`: descarrega el codi.
  2. `ruby/setup-ruby`: instal·la Ruby 3.3 i fa `bundle install` (amb caché).
  3. `actions/configure-pages`: obté el `base_path` de la web.
  4. `bundle exec jekyll build --baseurl ...`: genera `_site`.
  5. `actions/upload-pages-artifact`: puja `_site`.
- **Feina `deploy`:** `actions/deploy-pages` publica el que ha pujat `build`.

Detall important: GitHub Actions fa servir **Linux**, i per això hem hagut d'afegir la plataforma Linux al `Gemfile.lock` amb `bundle lock --add-platform x86_64-linux`.

Resultat de la primera execució: [OMPLIR: correcta / error]
[CAPTURA DE PANTALLA: pestanya Actions amb el workflow en verd]

## 17. Generació de la web

```bash
bundle exec jekyll build    # genera la web a _site/
bundle exec jekyll serve    # genera i serveix a http://localhost:4000
```

- **Fitxers font:** Markdown, `_config.yml`, `_layouts`, `_includes`, CSS i imatges.
- **Fitxers generats:** la carpeta `_site/`, amb un `index.html` per a cada pàgina, el CSS, les imatges, `feed.xml` i `sitemap.xml`.

Per exemple, `_pages/linux/permisos.md` es converteix en `_site/linux/permisos/index.html`.

`_site/` està al `.gitignore`, perquè és un resultat i es pot tornar a generar sempre.

[CAPTURA DE PANTALLA: sortida de `jekyll build` i contingut de `_site/`]

## 18. Problemes trobats

| # | Problema | On |
|---|---|---|
| 1 | La Ruby de macOS (2.6.10) és massa antiga per a Jekyll 4 | Ordinador de l'Adrià |
| 2 | [DESCRIURE PROBLEMA REAL SI N'HI HA] | |
| 3 | [DESCRIURE PROBLEMA REAL SI N'HI HA] | |

> Problemes típics que ens podríem trobar, per si ens passen: imatges o CSS que no carreguen a GitHub Pages (per culpa del `baseurl`), el workflow que falla a `bundle install` (per la plataforma Linux al `Gemfile.lock`), conflictes en fer `merge`. **Només s'han de posar a la taula si ens passen de veritat.**

## 19. Solucions

| # | Solució |
|---|---|
| 1 | Instal·lar una Ruby nova amb `chruby` + `ruby-install`, com diu la guia de Jekyll. [OMPLIR: confirmar que ho hem fet així] |
| 2 | [DESCRIURE SOLUCIÓ REAL] |
| 3 | [DESCRIURE SOLUCIÓ REAL] |

## 20. Proves

| Prova | Com | Resultat |
|---|---|---|
| La web es genera en local | `bundle exec jekyll build` | [OMPLIR] |
| La web es veu en local | `jekyll serve` + navegador | [OMPLIR] |
| El menú i totes les pàgines funcionen | Clicar totes les opcions | [OMPLIR] |
| Les imatges es veuen | Revisar les pàgines i els articles | [OMPLIR] |
| Versió mòbil | Eines de desenvolupador del navegador (F12) o mòbil | [OMPLIR] |
| El workflow funciona | Pestanya Actions | [OMPLIR] |
| La web publicada funciona | Obrir la URL de Pages | [OMPLIR] |
| **Actualització automàtica** | Canviar una pàgina → push → comprovar la web | [OMPLIR] |
| RSS i sitemap | Obrir `/feed.xml` i `/sitemap.xml` | [OMPLIR] |

**Prova d'actualització (apartat 14 de l'enunciat):**
- Canvi fet: [OMPLIR: quin fitxer i quin canvi]
- Commit: [OMPLIR: hash i missatge]
- Temps fins que s'ha vist publicat: [OMPLIR]
- [CAPTURA DE PANTALLA: abans]  [CAPTURA DE PANTALLA: després]

## 21. Resultat final

- Web publicada: [POSAR URL DE GITHUB PAGES]
- Repositori: [POSAR URL DEL REPOSITORI]
- 6 pàgines de documentació + 3 portades de secció, 3 articles, inici, blog, comparativa i qui som.
- Desplegament automàtic amb GitHub Actions.

[CAPTURA DE PANTALLA: pàgina d'inici publicada]

## 22. Conclusions

[OMPLIR: escriure-les al final amb les vostres paraules. Idees per començar:]

- Què us ha sorprès de Jekyll (per exemple, que tota la web siguin fitxers de text).
- Què ha estat el més difícil (instal·lació, `baseurl`, Git amb branques...).
- Si faríeu servir un SSG en una feina real, i per a què.

---

## Annex A: Qüestionari final

**1. Què és un Static Site Generator?**
És un programa que agafa contingut escrit normalment en Markdown, una configuració i unes plantilles, i genera una web completa en HTML, CSS i JS. La web resultant no necessita base de dades ni programació al servidor: només són fitxers que el servidor entrega tal qual.

**2. Quina diferència hi ha entre els fitxers Markdown i els fitxers HTML generats?**
Els Markdown són els **fitxers font**: els escrivim nosaltres, són fàcils de llegir i no porten disseny. Els HTML són els **fitxers generats**: els crea Jekyll a `_site/`, apliquen el tema i les plantilles, i són els que llegeix el navegador. Els Markdown es modifiquen; els HTML no, perquè es tornen a generar.

**3. Què és Git?**
És un sistema de control de versions. Guarda l'historial de canvis d'un projecte en *commits*, i així podem veure qui ha canviat què, tornar a una versió anterior o treballar en branques diferents alhora.

**4. Quina diferència hi ha entre Git i GitHub?**
Git és el programa que funciona al nostre ordinador. GitHub és un servei web que allotja repositoris Git a Internet i hi afegeix coses: col·laboradors, Pages, Actions, *issues*... Es pot fer servir Git sense GitHub, però no GitHub sense Git.

**5. Què és GitHub Pages?**
És un servei gratuït de GitHub que publica una web estàtica a partir d'un repositori, a una adreça com `https://usuari.github.io/repositori/`.

**6. Què passa quan executes `git push`?**
Els commits que tenim en local i no són a GitHub s'envien al repositori remot. En el nostre cas, si el push és a `main`, també s'engega el workflow de GitHub Actions, que torna a generar i publicar la web.

**7. Què passa quan modifiques un fitxer Markdown i tornes a generar la web?**
Jekyll torna a convertir el Markdown a HTML i substitueix els fitxers de `_site/`. Amb `jekyll serve` passa sol i només cal refrescar el navegador. A GitHub passa quan fem push: l'Action fa el `build` i publica la versió nova.

**8. Quines diferències has trobat entre la teva eina i les altres?**
Jekyll necessita Ruby i Bundler, i a macOS ens ha costat perquè la Ruby del sistema era antiga. Hugo és un sol executable i MkDocs només necessita `pip install`. Jekyll té el blog molt ben resolt (`_posts`), però el menú lateral de la documentació l'hem hagut de fer nosaltres, i MkDocs el fa sol a partir de `mkdocs.yml`. Hugo és més ràpid, però les seves plantilles semblen més complicades que Liquid. [OMPLIR: afegir alguna cosa que hàgiu notat vosaltres]

**9. Quins avantatges té una web estàtica?**
- **Seguretat:** no hi ha base de dades ni codi al servidor que es pugui atacar.
- **Rapidesa:** el servidor només entrega fitxers.
- **Cost:** es pot allotjar gratis (GitHub Pages, Netlify...).
- **Versions:** tot és text i es pot guardar a Git.
- **Poc manteniment:** no hi ha res a actualitzar al servidor.

**10. En quin tipus de projecte empresarial utilitzaries un generador de webs estàtiques?**
En documentació interna o d'un producte, manuals, una base de coneixement del departament d'IT, blogs tècnics, landing pages o webs corporatives petites que no canvien cada dia. No el faríem servir per a una botiga online o una web amb usuaris registrats.

**11. Quin SSG consideres més adequat per a documentació tècnica? Justifica-ho.**
**MkDocs**. Està pensat per a això: el menú lateral es genera sol, porta cercador inclòs i amb el tema Material queda molt bé sense gaire feina. Si la documentació fos enorme, Hugo també seria una bona opció per la velocitat.

**12. Quin SSG consideres més adequat per a una web corporativa? Justifica-ho.**
**Hugo**. És molt ràpid, té molts temes pensats per a webs d'empresa, admet diversos idiomes sense plugins i es pot fer pràcticament qualsevol disseny. Jekyll també serviria per a una web corporativa petita amb blog.

**13. Quina funció té GitHub Actions?**
Automatitzar tasques quan passa alguna cosa al repositori. En el nostre cas, quan fem push a `main` instal·la Ruby i Jekyll, genera la web i la publica a GitHub Pages sense que haguem de fer res més.

**14. Quins avantatges té automatitzar el desplegament?**
- No ens podem oblidar cap pas: sempre es fa igual.
- La web sempre està al dia amb el que hi ha a `main`.
- Si el *build* falla, ho veiem a la pestanya Actions i la web anterior continua funcionant.
- Qualsevol membre de l'equip pot publicar només fent push, sense tenir res especial instal·lat.
- Queda un registre de cada desplegament.

---

## Annex B: Repte final

*Una empresa necessita una web de documentació interna amb centenars de pàgines. Recomanaries un generador de webs estàtiques?*

**Sí, el recomanaríem.** Raons:

| Aspecte | Per què un SSG hi encaixa |
|---|---|
| **Seguretat** | Sense base de dades ni PHP, hi ha molt poc per atacar. Si és documentació interna, es pot servir dins la xarxa de l'empresa o darrere d'una autenticació. |
| **Rendiment** | Només són fitxers HTML, i carreguen molt ràpid encara que n'hi hagi centenars. |
| **Manteniment** | No s'ha d'actualitzar cap CMS ni cap plugin al servidor. |
| **Facilitat d'edició** | Markdown és fàcil d'aprendre per a tècnics. L'inconvenient és que algú que no sàpiga Git ho tindrà més difícil que amb un wiki. |
| **Desplegament** | Amb CI/CD (GitHub Actions, GitLab CI) cada canvi aprovat es publica sol. |
| **Escalabilitat** | Centenars de pàgines no són cap problema, sobretot amb Hugo o MkDocs. |
| **Dependències** | Només es necessiten per generar la web, no per servir-la. Hugo és un sol executable; MkDocs necessita Python. |
| **Cost** | Molt baix: un servidor web qualsevol (Nginx) o un servei gratuït. |

A més, tenir la documentació a Git permet revisar els canvis amb *pull requests* i saber qui ha canviat cada cosa.

**Quin triaríem: MkDocs** (amb el tema Material).
- Està fet específicament per a documentació tècnica.
- La navegació per seccions i el **cercador** vénen inclosos, i amb centenars de pàgines el cercador és imprescindible.
- És el més fàcil perquè altres companys del departament hi escriguin.

Si la web hagués de créixer fins a milers de pàgines, o necessités diversos idiomes, miraríem **Hugo** per la velocitat. **Jekyll** el deixaríem per a webs més petites o amb blog, com la d'aquesta pràctica.

---

## Annex C: Estat del projecte

| Element | Estat |
|---|---|
| Estructura Jekyll, configuració i plantilles | ✅ Configurat |
| Pàgines Linux, Xarxes i Serveis (2+ cadascuna) | ✅ Configurat |
| 3 articles amb imatge i comandes | ✅ Configurat |
| CSS propi, logo, favicon i layout propi | ✅ Configurat |
| RSS, sitemap, robots.txt, 404 | ✅ Configurat |
| Workflow de GitHub Actions | ✅ Configurat · ⏳ Pendent de comprovar |
| Instal·lació de Jekyll a l'ordinador de l'Adrià | ⏳ [OMPLIR] |
| Instal·lació de Jekyll a l'ordinador del Pere | ⏳ [OMPLIR] |
| `jekyll serve` funciona en local | ⏳ [OMPLIR] |
| Repositori a GitHub amb les branques `main`, `adria` i `pere` | ⏳ [OMPLIR] |
| Merges a `main` | ⏳ [OMPLIR] |
| GitHub Pages configurat (Source: GitHub Actions) | ⏳ [OMPLIR] |
| Web publicada i accessible | ⏳ [OMPLIR] |
| Prova d'actualització automàtica | ⏳ [OMPLIR] |
| PDF d'entrega | ⏳ [OMPLIR] |
