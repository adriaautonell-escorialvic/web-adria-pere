---
title: HTTP
permalink: /serveis/http/
layout: doc
section: serveis
order: 2
author: Pere
---

**HTTP** és el protocol que fan servir els navegadors per demanar pàgines web a un servidor. La versió segura, **HTTPS**, és el mateix però xifrat amb TLS.

| Protocol | Port | Xifrat |
|----------|:----:|:------:|
| HTTP | 80 | No |
| HTTPS | 443 | Sí |

## Com funciona

Sempre és igual: el client fa una **petició** i el servidor torna una **resposta**.

![Petició i resposta HTTP]({{ '/assets/images/http-peticio.svg' | relative_url }})

### Mètodes més habituals

- `GET` → demanar un recurs (una pàgina, una imatge...)
- `POST` → enviar dades (per exemple un formulari)
- `PUT` / `DELETE` → modificar o esborrar (sobretot en APIs)

### Codis d'estat

| Codi | Significat | Quan el veiem |
|:----:|------------|---------------|
| 200 | OK | Tot correcte |
| 301 | Moved Permanently | La pàgina ha canviat d'adreça (redirecció) |
| 403 | Forbidden | No tenim permís (sovint per permisos de fitxers!) |
| 404 | Not Found | El recurs no existeix |
| 500 | Internal Server Error | Error al servidor |

## Provar-ho amb curl

`curl` fa peticions HTTP des del terminal. És molt útil per comprovar un servidor sense navegador.

```bash
# Descarregar la pàgina i mostrar l'HTML
curl http://example.com

# Veure només les capçaleres de la resposta
curl -I http://example.com

# Veure tota la conversa (petició i resposta)
curl -v http://example.com
```

Amb `-I` veurem una cosa com `HTTP/1.1 200 OK` a la primera línia, i després capçaleres com `Content-Type`.

## Muntar un servidor web amb Apache

```bash
sudo apt update
sudo apt install apache2
systemctl status apache2
```

Si anem al navegador i posem la IP del servidor, hauria de sortir la pàgina de benvinguda d'Apache. Els fitxers de la web són a `/var/www/html`:

```bash
# Canviar la pàgina principal per una de nostra
echo "<h1>Servidor de proves de NetCore</h1>" | sudo tee /var/www/html/index.html

# Comprovar-ho des del mateix servidor
curl http://localhost
```

Els registres d'Apache són útils quan alguna cosa falla:

| Fitxer | Què hi ha |
|--------|-----------|
| `/var/log/apache2/access.log` | Totes les peticions que arriben |
| `/var/log/apache2/error.log` | Els errors |

```bash
# Anar veient les peticions en directe (Ctrl+C per sortir)
sudo tail -f /var/log/apache2/access.log
```

> **Nota:** aquesta mateixa web no fa servir Apache. És una web **estàtica** generada amb Jekyll i servida per GitHub Pages. Però la idea és la mateixa: el servidor només entrega fitxers HTML, CSS i imatges.
{: .consell }
