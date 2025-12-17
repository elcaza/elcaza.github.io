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
`docker-compose.yml`
~~~bash
# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      # DNS Ports
      - "53:53/tcp"
      - "53:53/udp"
      # Default HTTP Port
      - "80:80/tcp"
      # Default HTTPs Port. FTL will generate a self-signed certificate
      - "443:443/tcp"
      # Uncomment the line below if you are using Pi-hole as your DHCP server
      #- "67:67/udp"
      # Uncomment the line below if you are using Pi-hole as your NTP server
      #- "123:123/udp"
    environment:
      # Set the appropriate timezone for your location (https://en.wikipedia.org/wiki/List_of_tz_database_time_zones), e.g:
      TZ: 'Europe/London'
      # Set a password to access the web interface. Not setting one will result in a random password being assigned
      FTLCONF_webserver_api_password: 'YOUR_CUSTOM_SECURE_PASS'
      # If using Docker's default `bridge` network setting the dns listening mode should be set to 'ALL'
      FTLCONF_dns_listeningMode: 'ALL'
    # Volumes store your data between container upgrades
    volumes:
      # For persisting Pi-hole's databases and common configuration file
      - './etc-pihole:/etc/pihole'
      # Uncomment the below if you have custom dnsmasq config files that you want to persist. Not needed for most starting fresh with Pi-hole v6. If you're upgrading from v5 you and have used this directory before, you should keep it enabled for the first v6 container start to allow for a complete migration. It can be removed afterwards. Needs environment variable FTLCONF_misc_etc_dnsmasq_d: 'true'
      #- './etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      # See https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
      # Required if you are using Pi-hole as your DHCP server, else not needed
      - NET_ADMIN
      # Required if you are using Pi-hole as your NTP client to be able to set the host's system time
      - SYS_TIME
      # Optional, if Pi-hole should get some more processing time
      - SYS_NICE
    restart: unless-stopped
~~~
+ Recuerda cambiar: `FTLCONF_webserver_api_password`

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