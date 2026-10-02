---
title: Serveis
permalink: /serveis/
layout: doc
section: serveis
order: 0
author: Pere
---

Un **servei** és un programa que queda funcionant en segon pla i espera connexions (per això també se'n diu *dimoni* o *daemon*). En aquesta secció parlem dels dos que més fem servir.

## Pàgines d'aquesta secció

| Pàgina | Servei | Port |
|--------|--------|:----:|
| [SSH]({{ '/serveis/ssh/' | relative_url }}) | Accés remot segur al terminal | 22 |
| [HTTP]({{ '/serveis/http/' | relative_url }}) | Servidor web | 80 / 443 |

## Gestionar serveis amb systemctl

A Debian i Ubuntu els serveis es controlen amb `systemctl`. Serveix igual per a qualsevol servei, només canvia el nom:

```bash
# Veure si el servei funciona
systemctl status ssh

# Arrencar, aturar i reiniciar
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2

# Tornar a llegir la configuració sense aturar-lo
sudo systemctl reload apache2

# Que arrenqui sol quan s'engega el servidor
sudo systemctl enable apache2
```

Si un servei no arrenca, el primer que mirem són els registres (*logs*):

```bash
sudo journalctl -u apache2 --since "10 minutes ago"
```
