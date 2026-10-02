---
title: Linux
permalink: /linux/
layout: doc
section: linux
order: 0
author: Adrià Autonell
---

Gairebé tots els servidors que muntem a NetCore funcionen amb **Linux** (Debian o Ubuntu Server). En aquesta secció hi ha el bàsic per administrar-los des del terminal.

## Pàgines d'aquesta secció

| Pàgina | De què va |
|--------|-----------|
| [Usuaris i grups]({{ '/linux/usuaris-grups/' | relative_url }}) | Crear, modificar i esborrar usuaris. Afegir-los a grups. |
| [Permisos]({{ '/linux/permisos/' | relative_url }}) | Llegir els permisos amb `ls -l` i canviar-los amb `chmod` i `chown`. |

## Abans de començar

- Totes les comandes que comencen per `sudo` necessiten un usuari amb permisos d'administració.
- Si no saps què fa una comanda, mira'n el manual amb `man`:

```bash
man chmod
```

- Per sortir del manual es prem la tecla `q`.
