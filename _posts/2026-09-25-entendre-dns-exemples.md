---
title: "Entendre DNS amb exemples pràctics"
date: 2026-09-25 12:00:00 +0200
author: Pere
---

Tothom diu que "el DNS tradueix noms a IPs", però fins que no ho proves amb `dig` no acabes d'entendre què passa. En aquest article fem unes quantes proves des del terminal i expliquem què surt.

<!--more-->

![Resolució DNS pas a pas]({{ '/assets/images/dns-resolucio.svg' | relative_url }})

## Preparació

Necessitem la comanda `dig`. A Debian/Ubuntu:

```bash
sudo apt install dnsutils
```

## Prova 1: una consulta normal

```bash
dig example.com
```

La sortida és llarga, però hi ha tres parts que ens interessen:

- `status: NOERROR` → la consulta ha anat bé. Si surt `NXDOMAIN`, vol dir que el domini **no existeix**.
- `ANSWER SECTION` → aquí hi ha la resposta: el nom, el TTL, el tipus (`A`) i la IP.
- `SERVER` (al final) → quin servidor DNS ens ha respost.

Si només volem la IP:

```bash
dig +short example.com
```

## Prova 2: veure la memòria cau (TTL)

Executa la mateixa consulta dues vegades seguides i fixa't en el número que hi ha al costat del nom a la `ANSWER SECTION`:

```bash
dig example.com | grep -A1 "ANSWER SECTION"
```

La segona vegada el número és **més petit**. Això és el TTL que va baixant: el resolutor té la resposta guardada i ens la dona sense tornar a preguntar al servidor autoritatiu. Quan arribi a 0, la tornarà a demanar.

Per això quan es canvia la IP d'un domini, el canvi no es veu a tot arreu a l'instant.

## Prova 3: altres tipus de registre

```bash
# Quins servidors de correu té el domini
dig gmail.com MX +short

# Quins són els servidors DNS del domini
dig wikipedia.org NS +short
```

A la sortida dels `MX` surt un número davant de cada servidor. És la **prioritat**: com més baix, més preferència.

## Prova 4: seguir tot el camí

Aquesta és la més interessant. Amb `+trace`, `dig` no fa servir el resolutor i fa ell mateix tot el recorregut:

```bash
dig +trace www.wikipedia.org
```

Veurem els passos en ordre:

1. Els servidors **arrel** (`.`), que tenen noms com `a.root-servers.net`.
2. Els servidors del **TLD** (`org.`).
3. Els servidors **autoritatius** de `wikipedia.org`, que són els que finalment donen la resposta.

És exactament el que hi ha al dibuix de dalt.

## Prova 5: comparar dos servidors DNS

Podem preguntar el mateix a servidors diferents:

```bash
dig @8.8.8.8 example.com +short
dig @1.1.1.1 example.com +short
```

Normalment la resposta és la mateixa. Si no ho és, pot ser que un dels dos tingui encara la resposta antiga en memòria cau.

## I a /etc/hosts?

Abans de preguntar al DNS, Linux mira el fitxer `/etc/hosts`. Podem fer la prova afegint-hi una línia:

```bash
echo "127.0.0.1   provadns.local" | sudo tee -a /etc/hosts
ping -c 1 provadns.local
```

El `ping` va a `127.0.0.1` tot i que aquest domini no existeix enlloc. Això sí: **`dig` no mira `/etc/hosts`**, pregunta directament al servidor DNS. Per això `dig provadns.local` no el trobarà, tot i que el `ping` sí que funciona.

Després de la prova, recordeu treure la línia de `/etc/hosts`.

## Conclusió

Amb `dig` es veu molt clar que el DNS és una jerarquia: arrel → TLD → autoritatiu. I que el TTL i la memòria cau expliquen per què a vegades els canvis tarden a arribar.

Més teoria a la pàgina de [DNS]({{ '/xarxes/dns/' | relative_url }}).
