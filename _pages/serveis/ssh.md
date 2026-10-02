---
title: SSH
permalink: /serveis/ssh/
layout: doc
section: serveis
order: 1
author: Pere
---

**SSH** (*Secure Shell*) permet obrir un terminal d'un altre equip a través de la xarxa. Tot el que s'envia va **xifrat**, a diferència del Telnet antic, que enviava fins i tot les contrasenyes en text pla.

Fa servir el port **22/TCP** per defecte.

## Instal·lar el servidor

Al client (el nostre PC) normalment ja hi ha la comanda `ssh`. Al servidor cal instal·lar el paquet `openssh-server`:

```bash
sudo apt update
sudo apt install openssh-server
systemctl status ssh
```

## Connectar-se

```bash
# usuari@ip-del-servidor
ssh admin@192.168.1.10

# Si el servidor escolta en un altre port
ssh -p 2222 admin@192.168.1.10
```

La primera vegada surt un missatge preguntant si confiem en l'*empremta* (*fingerprint*) del servidor. Si diem `yes`, es guarda a `~/.ssh/known_hosts` i ja no ho torna a preguntar.

> **Compte:** si un dia surt un avís dient que l'empremta ha canviat (`REMOTE HOST IDENTIFICATION HAS CHANGED`), no ho ignoris. Pot ser que s'hagi reinstal·lat el servidor, però també pot ser algú fent-se passar per ell.
{: .avis }

## Copiar fitxers

```bash
# Del nostre PC al servidor
scp informe.pdf admin@192.168.1.10:/home/admin/

# Del servidor al nostre PC (el punt vol dir "aquí")
scp admin@192.168.1.10:/var/log/syslog .
```

## Autenticació amb claus

Entrar amb contrasenya funciona, però és més segur (i més còmode) fer servir **claus**. Es genera una parella de claus: la **privada** es queda al nostre PC i la **pública** es copia al servidor.

![Connexió SSH amb claus]({{ '/assets/images/ssh-claus.svg' | relative_url }})

```bash
# 1. Generar la parella de claus (al nostre PC)
ssh-keygen -t ed25519

# 2. Copiar la clau pública al servidor
ssh-copy-id admin@192.168.1.10

# 3. Provar-ho: ja no hauria de demanar la contrasenya del servidor
ssh admin@192.168.1.10
```

Si a `ssh-keygen` li posem una *passphrase*, la clau privada queda protegida per si algú ens la roba.

## Configuració del servidor

El fitxer de configuració és `/etc/ssh/sshd_config`. Algunes opcions típiques:

| Opció | Què fa |
|-------|--------|
| `Port 22` | Port on escolta el servidor |
| `PermitRootLogin no` | No deixa entrar directament com a root |
| `PasswordAuthentication no` | Només es pot entrar amb clau (no amb contrasenya) |

Després de canviar-lo, cal comprovar que no hi ha errors i reiniciar:

```bash
sudo sshd -t
sudo systemctl restart ssh
```

> **Consell:** abans de posar `PasswordAuthentication no`, comprova que ja pots entrar amb clau. I no tanquis la sessió oberta fins que no hagis provat d'entrar des d'una altra terminal. Si no, et pots quedar fora.
{: .consell }
