---
title: Permisos
permalink: /linux/permisos/
layout: doc
section: linux
order: 2
author: Adrià Autonell
---

Cada fitxer i carpeta de Linux té un **propietari**, un **grup** i uns **permisos** que diuen qui pot fer què.

## Com es llegeixen

Amb `ls -l` veiem els permisos:

```bash
ls -l
```

```text
-rwxr-xr-- 1 anna sistemes 220 set 20 10:15 script.sh
drwxr-x--- 2 anna sistemes 4096 set 20 10:16 informes
```

El primer bloc (`-rwxr-xr--`) és el que ens interessa:

![Com es llegeixen els permisos]({{ '/assets/images/permisos-rwx.svg' | relative_url }})

Els permisos es repeteixen tres vegades: per al **propietari**, per al **grup** i per als **altres** (la resta d'usuaris).

| Lletra | Valor | En un fitxer | En una carpeta |
|:------:|:-----:|--------------|----------------|
| `r` | 4 | Llegir el contingut | Veure'n la llista de fitxers |
| `w` | 2 | Modificar-lo | Crear o esborrar fitxers a dins |
| `x` | 1 | Executar-lo | *Entrar-hi* amb `cd` |

## Canviar permisos amb chmod

Hi ha dues maneres de fer-ho.

### En número (octal)

Se sumen els valors de cada grup de tres. Per exemple `rwx` = 4+2+1 = **7** i `r-x` = 4+0+1 = **5**.

```bash
# Propietari tot, grup i altres llegir i entrar
sudo chmod 755 /srv/web

# Només el propietari pot llegir i escriure (típic de fitxers amb contrasenyes)
chmod 600 config.conf
```

### Amb lletres (simbòlic)

És més còmode quan només vols canviar una cosa:

```bash
# Afegir permís d'execució al propietari
chmod u+x script.sh

# Treure el permís d'escriptura al grup i als altres
chmod go-w informe.txt
```

On `u` és l'usuari propietari, `g` el grup, `o` els altres i `a` tothom.

## Canviar propietari i grup

```bash
# Canviar el propietari
sudo chown anna informe.txt

# Canviar propietari i grup alhora
sudo chown anna:sistemes informe.txt

# Fer-ho a tota una carpeta i el que hi ha a dins (-R = recursiu)
sudo chown -R www-data:www-data /var/www/html
```

> **Consell:** evita fer `chmod 777`. Funciona, però vol dir que qualsevol usuari del servidor pot modificar el fitxer. Quasi sempre hi ha una opció millor, com posar el grup correcte.
{: .consell }

## Resum ràpid

| Permís | Número | Ús habitual |
|--------|:------:|-------------|
| `rwxr-xr-x` | 755 | Carpetes i scripts |
| `rw-r--r--` | 644 | Fitxers normals, pàgines web |
| `rw-------` | 600 | Claus privades, fitxers amb contrasenyes |
| `rwx------` | 700 | Carpeta `~/.ssh` |

Al [blog]({{ '/blog/' | relative_url }}) hi ha un article més pràctic sobre aquest tema, amb un cas real de carpeta compartida.
