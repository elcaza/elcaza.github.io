---
title: Pi-hole
published: 2025-12-16
description: 'Elimina los anuncios de tu red con Pi-hole y una raspberry pi'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/pi_hole/portada.png'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://pi-hole.net/" target="_blank">Sitio web oficial</a>
+ <a href="https://github.com/pi-hole/pi-hole/" target="_blank">Repositorio de Github</a>
+ <a href="https://github.com/pi-hole/docker-pi-hole/" target="_blank">Docker</a>

# Uso en Docker

## Añade el siguiente archivo 

### ENV
`.env`
~~~bash
# Configuración de Red y Tiempo
PIHOLE_TZ=America/Mexico_City

# Seguridad
PIHOLE_PASSWORD=tu_pass

# Configuración FTL
PIHOLE_DNS_MODE=ALL

# Logs de Docker
LOG_MAX_SIZE=10m
LOG_MAX_FILES=3
~~~
+ <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target="_blank">Zonas horarias</a>

### Docker compose
`docker-compose.yml`
~~~bash
# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    dns:
      - 1.1.1.1
      - 8.8.8.8
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
      - "443:443/tcp"
      #- "67:67/udp"
      #- "123:123/udp"
    environment:
      TZ: '${PIHOLE_TZ}'
      FTLCONF_webserver_api_password: '${PIHOLE_PASSWORD}'
      FTLCONF_dns_listeningMode: '${PIHOLE_DNS_MODE}'
    volumes:
      - './etc-pihole:/etc/pihole'
      - '/etc/localtime:/etc/localtime:ro'
      - '/etc/timezone:/etc/timezone:ro'
    cap_add:
      - NET_ADMIN
      - SYS_NICE
    logging:
      driver: "json-file"
      options:
        max-size: "${LOG_MAX_SIZE}"
        max-file: "${LOG_MAX_FILES}"
    restart: unless-stopped
~~~

## Activarlo

~~~bash
docker compose up -d
~~~

## Desactivarlo 

~~~bash
docker compose down
~~~

# Haz que tu router resuelva hacia tu pi-hole

1. Entra a tu Router: Abre tu navegador y ve a http://192.168.0.1 (o la IP de tu TP-Link).
1. Inicia Sesión: Introduce tu contraseña de administrador.
1. Ve a la pestaña Advanced: En el menú superior.
1. Navega a Network > DHCP Server: En la barra lateral izquierda.
1. Configura el DNS:
1. Busca los campos Primary DNS y Secondary DNS.
1. En Primary DNS, escribe la IP estática de tu Raspberry Pi.
1. En Secondary DNS, déjalo en 0.0.0.0 (o vacío). No pongas el de Google (8.8.8.8) aquí, porque si lo haces, los dispositivos podrían saltarse el Pi-hole.
1. Guarda los cambios: Haz clic en Save.
1. Reinicia tu router o vuelve a conectar tus dispositivos

Nota:
+ Pon la IP de tu Raspberry Pi en ambos campos (Primario y Secundario).
+ Según Gemini: Jamás pongas un DNS público (como 1.1.1.1) como secundario en el router, porque los sistemas operativos modernos suelen rotar entre ellos y terminarás viendo anuncios el 50% del tiempo.

:::note[Nota final]
¡Gracias por terminar de leer este artículo! uwur

— El Capitán

¿Tienes alguna duda o te gustaría comentar algo sobre este artículo?
+ <a href="https://t.me/elcazablog" target="_blank">Únete a nuestra comunidad en Telegram</a>

Puedes encontrarme en:
+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>
+ <a href="https://www.linkedin.com/in/elcaza/" target="_blank">LinkedIn</a>
:::