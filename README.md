# NetCore Systems: portal de documentació tècnica

Web estàtica de documentació tècnica per al departament de sistemes d'una empresa fictícia (**NetCore Systems**), feta amb **Jekyll** i publicada amb **GitHub Pages**.

Pràctica de *Creació i publicació d'una web estàtica amb GitHub Pages*: Implantació d'aplicacions web, ASIX 2n curs.

| | |
|---|---|
| **Autors** | Adrià Autonell i Pere [OMPLIR cognom] |
| **Eina** | Jekyll (Static Site Generator) |
| **Versió de Jekyll** | [OMPLIR: resultat de `bundle exec jekyll -v`] |
| **Versió de Ruby** | [OMPLIR: resultat de `ruby -v`] |
| **Tema** | minima 2.5, amb plantilles i CSS propis |
| **Repositori** | [POSAR URL DEL REPOSITORI] |
| **Web publicada** | [POSAR URL DE GITHUB PAGES] |

---

## 1. Descripció

La web és un petit portal on els administradors de sistemes poden consultar guies ràpides. Té:

- **Inici:** presentació de l'empresa, objectius i enllaços a les seccions.
- **Linux:** Usuaris i grups, Permisos.
- **Xarxes:** TCP/IP, DNS.
- **Serveis:** SSH, HTTP.
- **Blog:** 3 articles amb casos pràctics.
- **Comparativa** entre Jekyll, Hugo i MkDocs.
- **Qui som.**

Tot el contingut està escrit en **Markdown** i Jekyll el converteix en HTML.

## 2. Comparativa breu amb els altres SSG

| | Jekyll | Hugo | MkDocs |
|---|---|---|---|
| Llenguatge | Ruby | Go | Python |
| Facilitat | Fàcil | Mitjana | Molt fàcil |
| Velocitat | Correcta | Molt ràpid | Ràpid |
| Punt fort | Blogs i webs senzilles | Webs grans, rapidesa | Documentació tècnica |
| GitHub Pages | Integrat de sèrie | Amb GitHub Actions | `mkdocs gh-deploy` o Actions |

La comparativa completa és a la pàgina [/comparativa/](_pages/comparativa.md) de la web i a la memòria.

## 3. Per què Jekyll

- Era l'eina que ens tocava, però a més encaixa bé amb el projecte, que té una part de **blog** i una de **documentació**.
- És el generador que GitHub Pages fa servir per defecte, i hi ha molta documentació sobre com publicar-lo.
- L'estructura de carpetes (`_posts`, `_layouts`, `_includes`) és fàcil d'entendre i d'explicar.

## 4. Instal·lació

Cal tenir **Ruby** (amb les eines per compilar), **Bundler** i **Git**.

### macOS

La Ruby que porta macOS és antiga (a l'ordinador de l'Adrià era la 2.6.10) i Jekyll recomana no fer-la servir. Seguint la [guia oficial de Jekyll per a macOS](https://jekyllrb.com/docs/installation/macos/):

```bash
brew install chruby ruby-install
ruby-install ruby 3.3

# Afegir al final de ~/.zshrc
echo 'source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh' >> ~/.zshrc
echo 'source $(brew --prefix)/opt/chruby/share/chruby/auto.sh' >> ~/.zshrc
echo 'chruby ruby-3.3' >> ~/.zshrc
```

Tanquem i tornem a obrir el terminal i comprovem:

```bash
ruby -v
gem install bundler
```

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install ruby-full build-essential zlib1g-dev git
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
gem install bundler
```

### Windows

Instal·lar [RubyInstaller](https://rubyinstaller.org/) (la versió *with Devkit*) i després `gem install bundler`.

### Dependències del projecte

Des de la carpeta del projecte:

```bash
bundle install
```

Això llegeix el `Gemfile` i instal·la Jekyll, el tema minima i els plugins. També crea el fitxer `Gemfile.lock` amb les versions exactes.

Com que GitHub Actions construeix la web en Linux, hem d'afegir aquesta plataforma al `Gemfile.lock` (només s'ha de fer una vegada):

```bash
bundle lock --add-platform x86_64-linux
```

## 5. Execució local

```bash
bundle exec jekyll serve
```

La web queda a <http://localhost:4000>. Mentre el servidor està en marxa, si modifiquem un fitxer Markdown, Jekyll torna a generar la web sol i només cal refrescar el navegador.

> Si modifiquem `_config.yml` cal aturar el servidor (`Ctrl+C`) i tornar-lo a engegar.

## 6. Generació de la web

```bash
bundle exec jekyll build
```

Jekyll genera la web a la carpeta **`_site/`**: HTML, CSS, imatges, `feed.xml` i `sitemap.xml`. Aquesta carpeta és el resultat final i **no es puja al repositori** (és al `.gitignore`), perquè es pot tornar a generar quan vulguem.

- **Fitxers font:** Markdown, `_config.yml`, plantilles (`_layouts`, `_includes`), CSS i imatges.
- **Fitxers generats:** el que hi ha a `_site/`, que és el que realment veu el navegador.

## 7. Publicació amb GitHub Pages

1. Pujar el projecte a GitHub (`git push`).
2. Al repositori: **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. A cada `git push` a `main` s'executa el workflow i la web es publica a:

   [POSAR URL DE GITHUB PAGES]

L'adreça segueix el format `https://USUARI.github.io/NOM-DEL-REPOSITORI/`.

## 8. Desplegament automàtic amb GitHub Actions

El workflow és a [`.github/workflows/jekyll.yml`](.github/workflows/jekyll.yml).

```text
git push a main
      ↓
GitHub Actions engega el workflow
      ↓
build:  descarrega el codi → instal·la Ruby 3.3 i les gems → jekyll build → puja _site
      ↓
deploy: publica _site a GitHub Pages
      ↓
Web actualitzada
```

- **Quan s'executa:** a cada `push` a la branca `main`, o a mà des de la pestanya *Actions* (`workflow_dispatch`).
- **Eina:** Jekyll, instal·lat amb `ruby/setup-ruby` i `bundle install`.
- **Com es genera:** `bundle exec jekyll build --baseurl "/NOM-DEL-REPOSITORI"`. El `baseurl` el calcula sol l'acció `configure-pages`.
- **Com es publica:** `upload-pages-artifact` puja la carpeta `_site` i `deploy-pages` la publica.

L'estat de cada execució es pot veure a la pestanya **Actions** del repositori.

## 9. Estructura del projecte

```text
.
├── .github/workflows/jekyll.yml   # Workflow de GitHub Actions
├── _config.yml                    # Configuració de Jekyll
├── _data/navigation.yml           # Opcions del menú
├── _includes/                     # Trossos d'HTML reutilitzables
│   ├── head.html                  #   <head>: favicon i CSS propi
│   ├── header.html                #   capçalera amb logo i menú
│   ├── footer.html                #   peu de pàgina
│   └── section-nav.html           #   menú lateral de cada secció
├── _layouts/
│   └── doc.html                   # Plantilla de les pàgines de documentació
├── _pages/                        # Pàgines (Markdown)
│   ├── linux/                     #   index, usuaris-grups, permisos
│   ├── xarxes/                    #   index, tcp-ip, dns
│   ├── serveis/                   #   index, ssh, http
│   ├── blog.md
│   ├── comparativa.md
│   └── qui-som.md
├── _posts/                        # Articles del blog (AAAA-MM-DD-titol.md)
├── assets/
│   ├── css/netcore.css            # CSS propi
│   └── images/                    # Logo, favicon i diagrames SVG
├── 404.html                       # Pàgina d'error
├── index.md                       # Pàgina d'inici
├── robots.txt
├── Gemfile                        # Gems que necessita el projecte
├── MEMORIA.md                     # Memòria de la pràctica
└── README.md
```

Les carpetes que comencen per `_` són especials per a Jekyll i no es copien tal qual a la web. `_pages` no és una carpeta estàndard, i per això l'hem posat a `include` al `_config.yml`.

## 10. URL de la web publicada

https://adriaautonell-escorialvic.github.io