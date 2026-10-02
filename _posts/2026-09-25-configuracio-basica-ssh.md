---
title: "Configuració bàsica d'un servidor SSH"
date: 2026-09-25 16:00:00 +0200
author: Pere
---

En aquest article muntem un servidor SSH des de zero en una màquina virtual amb Ubuntu Server, i el deixem una mica més segur que com ve per defecte: entrada amb claus i sense accés directe de root.

<!--more-->

![Connexió SSH amb claus]({{ '/assets/images/ssh-claus.svg' | relative_url }})

## L'escenari

| Equip | Sistema | IP |
|-------|---------|----|
| Servidor | Ubuntu Server (màquina virtual) | `192.168.1.10` |
| Client | El nostre PC | la que tingui |

A la màquina virtual hem posat la xarxa en mode **adaptador pont** perquè tingui una IP de la mateixa xarxa que el PC. La IP d'exemple és `192.168.1.10`; cadascú haurà de posar la seva (la podeu veure amb `ip addr`).

## Pas 1: instal·lar el servidor

Al servidor:

```bash
sudo apt update
sudo apt install openssh-server
systemctl status ssh
```

Ha de sortir `active (running)` en verd. Comprovem també que escolta al port 22:

```bash
sudo ss -tlnp | grep :22
```

## Pas 2: primera connexió amb contrasenya

Des del PC:

```bash
ssh admin@192.168.1.10
```

Ens preguntarà si confiem en l'empremta del servidor. Diem `yes` i posem la contrasenya de l'usuari `admin` del servidor. Si veiem el *prompt* del servidor, ja hi som. Sortim amb `exit`.

## Pas 3: crear les claus

Al **PC** (no al servidor):

```bash
ssh-keygen -t ed25519 -C "admin@netcore"
```

- Ens pregunta on guardar-la: Enter per deixar-la a `~/.ssh/id_ed25519`.
- Ens demana una *passphrase*. És recomanable posar-ne una.

Això crea dos fitxers:

- `~/.ssh/id_ed25519` → la clau **privada**. No s'ha de passar a ningú.
- `~/.ssh/id_ed25519.pub` → la clau **pública**. Aquesta sí que es copia als servidors.

## Pas 4: copiar la clau pública al servidor

```bash
ssh-copy-id admin@192.168.1.10
```

Aquesta comanda afegeix la nostra clau pública al fitxer `~/.ssh/authorized_keys` de l'usuari `admin` al servidor. Ara provem:

```bash
ssh admin@192.168.1.10
```

Si hem posat *passphrase*, ens la demana (és la de la clau, no la del servidor). Si no n'hem posat, entrem directament.

## Pas 5: fer el servidor més segur

Al servidor, fem primer una còpia de la configuració, per si de cas:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
```

Busquem aquestes línies (algunes estan comentades amb `#`) i les deixem així:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

- **PermitRootLogin no:** ningú pot entrar directament com a root. Si cal, s'entra amb un usuari normal i es fa `sudo`.
- **PasswordAuthentication no:** ja no es pot entrar amb contrasenya, només amb clau. Així els atacs de provar contrasenyes no serveixen de res.

> **Compte:** a Ubuntu hi pot haver fitxers a `/etc/ssh/sshd_config.d/` que tornin a activar `PasswordAuthentication`. Si després de reiniciar encara et deixa entrar amb contrasenya, mira-hi.
{: .avis }

Comprovem que no hi ha cap error de sintaxi i reiniciem:

```bash
sudo sshd -t
sudo systemctl restart ssh
```

Si `sshd -t` no diu res, és que està bé.

## Pas 6: comprovar-ho

**Sense tancar la sessió que tenim oberta**, obrim un altre terminal al PC i provem:

```bash
# Amb la clau: ha de funcionar
ssh admin@192.168.1.10

# Forçant contrasenya: ha de fallar amb "Permission denied (publickey)"
ssh -o PubkeyAuthentication=no admin@192.168.1.10
```

Si les dues coses passen com esperàvem, ja està.

## Resum

| Pas | Comanda clau |
|-----|--------------|
| Instal·lar | `apt install openssh-server` |
| Generar claus | `ssh-keygen -t ed25519` |
| Copiar la clau | `ssh-copy-id usuari@ip` |
| Configurar | `/etc/ssh/sshd_config` |
| Comprovar i reiniciar | `sshd -t` i `systemctl restart ssh` |

Més detalls a la pàgina de [SSH]({{ '/serveis/ssh/' | relative_url }}).
