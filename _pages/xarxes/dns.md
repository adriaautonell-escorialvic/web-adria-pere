---
title: DNS
permalink: /xarxes/dns/
layout: doc
section: xarxes
order: 2
author: Pere
---

El **DNS** (*Domain Name System*) és el que tradueix noms com `www.example.com` a adreces IP. Nosaltres recordem noms, però els equips necessiten la IP per connectar-se.

![Procés de resolució DNS]({{ '/assets/images/dns-resolucio.svg' | relative_url }})

## Com funciona (resumit)

1. L'equip pregunta al seu **resolutor DNS** (el router, el de l'operador o un de públic com `8.8.8.8`).
2. Si el resolutor no ho té guardat, pregunta als **servidors arrel**.
3. L'arrel l'envia als servidors del **TLD** (`.com`, `.cat`, `.org`...).
4. El TLD l'envia al **servidor autoritatiu** del domini, que és qui té la resposta.
5. El resolutor ens torna la IP i la guarda en memòria cau durant un temps (el **TTL**).

## Tipus de registres

| Registre | Per a què serveix | Exemple |
|----------|-------------------|---------|
| `A` | Nom → IPv4 | `www → 203.0.113.10` |
| `AAAA` | Nom → IPv6 | `www → 2001:db8::10` |
| `CNAME` | Un nom que apunta a un altre nom | `web → www.example.com` |
| `MX` | Servidor de correu del domini | `10 mail.example.com` |
| `NS` | Servidors DNS del domini | `ns1.example.com` |
| `TXT` | Text lliure (verificacions, SPF...) | `"v=spf1 ..."` |

## Comprovar el DNS

### dig

És la comanda més completa. A Debian/Ubuntu ve al paquet `dnsutils` (o `bind9-dnsutils`):

```bash
sudo apt install dnsutils
```

```bash
# Consulta normal (registre A)
dig example.com

# Només la resposta, sense tota la informació extra
dig +short example.com

# Preguntar per un tipus concret de registre
dig example.com MX

# Preguntar a un servidor DNS concret
dig @1.1.1.1 example.com
```

A la sortida de `dig` normal, la part important és la `ANSWER SECTION`. El número que surt al costat del nom és el TTL en segons.

### nslookup

Més senzilla, i també existeix a Windows:

```bash
nslookup example.com
```

## Fitxers de configuració a Linux

| Fitxer | Per a què serveix |
|--------|-------------------|
| `/etc/hosts` | Noms que es resolen "a mà", sense preguntar al DNS. Té prioritat. |
| `/etc/resolv.conf` | Quin servidor DNS fa servir l'equip |

Per exemple, per fer proves amb un servidor de la xarxa local sense tenir DNS propi, podem afegir a `/etc/hosts`:

```text
192.168.1.20   intranet.netcore.local
```

> **Consell:** a Ubuntu, `/etc/resolv.conf` sovint apunta a `127.0.0.53`. Això és el `systemd-resolved`, un resolutor local. Per veure quin DNS real està fent servir: `resolvectl status`.
{: .consell }

Al blog hi ha un article amb més exemples pràctics: *Entendre DNS amb exemples pràctics*.
