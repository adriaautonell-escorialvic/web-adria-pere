---
title: TCP/IP
permalink: /xarxes/tcp-ip/
layout: doc
section: xarxes
order: 1
author: Pere
---

**TCP/IP** és el conjunt de protocols que fa servir Internet (i gairebé qualsevol xarxa local). Es divideix en capes, i cada capa s'encarrega d'una part de la feina.

![Model TCP/IP de quatre capes]({{ '/assets/images/tcpip-capes.svg' | relative_url }})

## Les quatre capes

| Capa | Què fa | Exemples |
|------|--------|----------|
| Aplicació | El que veu l'usuari o el programa | HTTP, DNS, SSH |
| Transport | Porta les dades d'un programa a un altre fent servir **ports** | TCP, UDP |
| Internet | Porta els paquets d'un equip a un altre fent servir **adreces IP** | IP, ICMP |
| Accés a la xarxa | Envia els bits pel cable o la Wi-Fi | Ethernet, Wi-Fi |

## Adreces IP

Una adreça IPv4 té 4 números de 0 a 255, per exemple `192.168.1.10`. Sempre va acompanyada d'una **màscara**, que diu quina part és la xarxa i quina l'equip.

- `192.168.1.10/24` → la xarxa és `192.168.1.0` i hi caben 254 equips.
- Les adreces `10.x.x.x`, `172.16-31.x.x` i `192.168.x.x` són **privades**: només funcionen dins la xarxa local.

Per veure la IP del nostre equip:

```bash
ip addr
```

A la sortida busquem la interfície (per exemple `enp0s3` o `eth0`) i la línia `inet`, que és la IPv4. La interfície `lo` (127.0.0.1) és el *loopback*, l'equip parlant amb ell mateix.

Per veure la ruta per defecte, és a dir, la porta d'enllaç:

```bash
ip route
```

La línia que comença per `default via` ens diu la IP del router.

## TCP i UDP

| | TCP | UDP |
|---|-----|-----|
| Connexió | Sí, primer es fa la "salutació" (*three-way handshake*) | No |
| Fiable | Sí, si es perd un paquet es torna a enviar | No |
| Velocitat | Més lent | Més ràpid |
| Exemples | HTTP, SSH, correu | DNS, streaming, jocs |

### Ports que hem de conèixer

| Port | Protocol | Servei |
|:----:|:--------:|--------|
| 22 | TCP | SSH |
| 53 | UDP/TCP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |

Per veure quins ports està escoltant el nostre servidor:

```bash
sudo ss -tulpn
```

`-t` TCP, `-u` UDP, `-l` només els que escolten, `-p` quin programa és i `-n` números en lloc de noms.

## Comandes de diagnòstic

```bash
# Comprovar si arribem a un equip (Ctrl+C per parar)
ping 8.8.8.8

# Enviar només 4 paquets
ping -c 4 192.168.1.1

# Veure per quins routers passen els paquets
traceroute 8.8.8.8
```

Si `traceroute` no està instal·lat: `sudo apt install traceroute`.

> **Compte:** que un `ping` no respongui no sempre vol dir que l'equip estigui apagat. Molts tallafocs bloquegen l'ICMP.
{: .avis }
