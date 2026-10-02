---
title: Usuaris i grups
permalink: /linux/usuaris-grups/
layout: doc
section: linux
order: 1
author: Adrià Autonell
---

A Linux cada persona (o servei) que fa servir el sistema té un **usuari**. Els usuaris s'agrupen en **grups**, i així és més fàcil donar permisos a moltes persones de cop.

![Esquema d'usuaris i grups]({{ '/assets/images/usuaris-grups.svg' | relative_url }})

## On es guarda la informació

| Fitxer | Què hi ha |
|--------|-----------|
| `/etc/passwd` | La llista d'usuaris (nom, UID, carpeta personal, shell...) |
| `/etc/shadow` | Les contrasenyes xifrades (només el pot llegir root) |
| `/etc/group` | La llista de grups i qui hi ha a cada grup |

Per exemple, per veure la línia d'un usuari concret:

```bash
grep anna /etc/passwd
```

Surt una cosa així: `anna:x:1001:1001:Anna Puig,,,:/home/anna:/bin/bash`. Els camps van separats per `:` i el més important és el **UID** (1001), la **carpeta personal** i el **shell**.

## Crear usuaris

A Debian/Ubuntu hi ha dues comandes i sovint es confonen:

- `adduser` → és *interactiva*: et pregunta la contrasenya i les dades, i et crea la carpeta personal. És la que fem servir normalment.
- `useradd` → és la de baix nivell. Si no li passes opcions, **no** crea la carpeta personal.

```bash
# Forma fàcil (pregunta la contrasenya i les dades)
sudo adduser anna

# Amb useradd: -m crea /home/marc i -s li posa bash com a shell
sudo useradd -m -s /bin/bash marc
sudo passwd marc
```

## Grups

```bash
# Crear un grup nou
sudo addgroup sistemes

# Afegir un usuari a un grup SENSE treure'l dels altres
sudo usermod -aG sistemes anna

# Veure a quins grups està un usuari
groups anna
id anna
```

> **Compte amb `usermod -G`:** si et deixes la `-a` (*append*), l'usuari surt de tots els altres grups secundaris i només queda al que has posat. És un error molt típic.
{: .avis }

Els canvis de grup no s'apliquen a la sessió que ja està oberta. L'usuari ha de tancar sessió i tornar a entrar (o fer `newgrp sistemes`).

### Donar permisos d'administrador

A Ubuntu i Debian, els usuaris del grup `sudo` poden executar comandes com a root:

```bash
sudo usermod -aG sudo anna
```

## Modificar i esborrar usuaris

```bash
# Bloquejar un usuari (no podrà iniciar sessió)
sudo usermod -L marc

# Desbloquejar-lo
sudo usermod -U marc

# Esborrar l'usuari però deixar la seva carpeta
sudo deluser marc

# Esborrar l'usuari i també la carpeta personal
sudo deluser --remove-home marc
```

## Comandes útils per saber qui hi ha

| Comanda | Per a què serveix |
|---------|-------------------|
| `whoami` | Diu amb quin usuari estàs treballant |
| `who` | Mostra qui té una sessió oberta ara mateix |
| `last` | Historial dels últims inicis de sessió |
| `id usuari` | UID, GID i grups d'un usuari |

## Més informació

- Manual de Debian sobre usuaris: [Debian Reference, cap. 4](https://www.debian.org/doc/manuals/debian-reference/ch04.en.html)
- `man adduser` i `man usermod`
