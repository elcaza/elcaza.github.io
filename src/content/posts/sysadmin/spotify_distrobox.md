---
title: Spotify con distrobox
published: 2026-01-09
description: 'Crea tu propia nube virtual, al estilo Google Drive con NextCloud'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/spotify_distrobox/portada.jpg'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://distrobox.it/" target="_blank">Distrobox</a>
+ <a href="https://www.spotify.com/es/download/linux/" target="_blank">Spotify para Linux</a>

# El problema 

La nueva versión de Spotify para Linux (en este caso Debian) requiere `GLIBC_2.38`, dando como error:

~~~bash
/lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.38' not found (required by ./spotify)
~~~

# La solución

Utilizar Distrobox para instalar Spotify

## Instala podman y distrobox

~~~bash
sudo apt install podman distrobox -y
~~~

## Crea una imagen de archlinux:latest
~~~bash
distrobox create --name spotify --image docker.io/library/archlinux:latest

distrobox list
~~~

## Entra a tu imagen 
~~~bash
distrobox enter spotify
~~~

## Actualiza los paquetes e instala Spotify
~~~bash
sudo pacman -Syu

sudo pacman -S spotify-launcher libcurl-gnutls libmms --noconfirm

# calidad - OPCIONAL
sudo pacman -S libpulse pipewire-alsa pipewire-pulse --noconfirm
~~~

## Ejecuta la aplicación y corrobora que todo funciona correctamente
~~~bash
spotify-launcher
~~~

## Añade un icono para lanzar la aplicación desde tu entorno gráfico

~~~bash
# Default sin nombre
# distrobox-export --app spotify-launcher

# Si quieres añadir un nombre en especifico
distrobox-export --app spotify-launcher --extra-flags "--name Spotify"
~~~

## Actualizar tu contenedor y la aplicación
~~~bash
distrobox upgrade spotify
~~~

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