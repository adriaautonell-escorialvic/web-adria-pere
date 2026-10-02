---
title: Xarxes
permalink: /xarxes/
layout: doc
section: xarxes
order: 0
author: Pere
---

Si un servidor no té xarxa, no serveix de gaire. Aquí expliquem com es comuniquen els equips i les comandes que fem servir per veure si la xarxa funciona.

## Pàgines d'aquesta secció

| Pàgina | De què va |
|--------|-----------|
| [TCP/IP]({{ '/xarxes/tcp-ip/' | relative_url }}) | Les capes del model, adreces IP, ports i comandes de diagnòstic. |
| [DNS]({{ '/xarxes/dns/' | relative_url }}) | Com es passa d'un nom de domini a una IP i com es comprova. |

## Quan alguna cosa no va

Normalment seguim aquest ordre, de baix cap a dalt:

1. Té IP l'equip? → `ip addr`
2. Arriba a la porta d'enllaç (el router)? → `ping` a la IP del router
3. Arriba a Internet? → `ping 8.8.8.8`
4. Resol noms? → `ping google.com` o `dig google.com`

Si el pas 3 funciona però el 4 no, el problema és el **DNS**.
