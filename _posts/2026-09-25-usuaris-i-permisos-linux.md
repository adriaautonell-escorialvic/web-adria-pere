---
title: "Com gestionar usuaris i permisos a Linux"
date: 2026-09-25 10:00:00 +0200
author: Adrià Autonell
---

Aquest article és un cas pràctic. Ens han demanat preparar una carpeta compartida per a l'equip de sistemes: tres persones hi han de poder treballar, però la resta d'usuaris del servidor no hi han de poder entrar. Ho farem pas a pas.

<!--more-->

![Usuaris i grups]({{ '/assets/images/usuaris-grups.svg' | relative_url }})

## El que volem aconseguir

- Tres usuaris: `anna`, `marc` i `laia`.
- Un grup `sistemes` amb els tres a dins.
- Una carpeta `/srv/sistemes` on **només** el grup `sistemes` pugui llegir i escriure.

## Pas 1: crear els usuaris

```bash
sudo adduser anna
sudo adduser marc
sudo adduser laia
```

`adduser` ens anirà preguntant la contrasenya i el nom complet. La resta de preguntes (telèfon, habitació...) es poden deixar en blanc prement Enter.

Per comprovar que s'han creat:

```bash
tail -n 3 /etc/passwd
```

Hauríem de veure les tres línies noves al final.

## Pas 2: crear el grup i afegir-hi els usuaris

```bash
sudo addgroup sistemes
sudo usermod -aG sistemes anna
sudo usermod -aG sistemes marc
sudo usermod -aG sistemes laia
```

I comprovem-ho:

```bash
getent group sistemes
```

Ha de sortir una cosa com `sistemes:x:1004:anna,marc,laia`.

Recordeu la `-a` de `usermod -aG`. Sense ella, l'usuari perd els grups que ja tenia.

## Pas 3: crear la carpeta i posar-li el grup

```bash
sudo mkdir /srv/sistemes
sudo chown root:sistemes /srv/sistemes
```

Ara el propietari és `root` i el grup és `sistemes`.

## Pas 4: posar els permisos

Volem que:

| Qui | Permisos | Valor |
|-----|----------|:-----:|
| Propietari (root) | llegir, escriure, entrar | 7 |
| Grup (sistemes) | llegir, escriure, entrar | 7 |
| Altres | res | 0 |

```bash
sudo chmod 770 /srv/sistemes
ls -ld /srv/sistemes
```

La sortida de `ls -ld` ha de començar per `drwxrwx---`.

![Com es llegeixen els permisos]({{ '/assets/images/permisos-rwx.svg' | relative_url }})

## Pas 5: el problema dels fitxers nous

Aquí hi ha un detall que és fàcil passar per alt. Quan l'`anna` crea un fitxer a la carpeta, el fitxer queda amb el **grup principal de l'anna** (`anna`), no amb el grup `sistemes`. Això vol dir que potser en `marc` no el pot modificar.

La solució és activar el bit **setgid** a la carpeta. Amb aquest bit, tot el que es crea a dins hereta el grup de la carpeta:

```bash
sudo chmod g+s /srv/sistemes
ls -ld /srv/sistemes
```

Ara els permisos es veuen com `drwxrws---`. Aquella `s` en lloc de la `x` del grup és el setgid.

## Pas 6: provar-ho

La millor manera de saber si funciona és provar-ho amb els usuaris de veritat:

```bash
# Com a anna: crear un fitxer
sudo -u anna touch /srv/sistemes/prova.txt

# Com a marc: escriure-hi
sudo -u marc sh -c 'echo "hola" >> /srv/sistemes/prova.txt'

# Com un usuari que no és del grup (hauria de fallar)
sudo -u nobody ls /srv/sistemes
```

L'última comanda ha de donar `Permission denied`. Si és així, tot està bé.

## Resum

1. Crear usuaris amb `adduser`.
2. Crear el grup i afegir-hi els usuaris amb `usermod -aG`.
3. Canviar el grup de la carpeta amb `chown`.
4. Posar permisos `770` amb `chmod`.
5. Activar el setgid amb `chmod g+s` perquè els fitxers nous tinguin el grup correcte.

Tot això està explicat amb més detall a les pàgines de [Usuaris i grups]({{ '/linux/usuaris-grups/' | relative_url }}) i [Permisos]({{ '/linux/permisos/' | relative_url }}).
