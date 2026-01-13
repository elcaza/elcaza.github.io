---
title: TailScale
published: 2025-12-21
description: 'Conecta tus dispositivos a través de una VPN con TailScale'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/tailscale/portada.jpg'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://tailscale.com/" target="_blank">Sitio web oficial</a>
+ <a href="https://tailscale.com/pricing" target="_blank">Planes y precios</a>
    + A la fecha de publicación de este post, plan gratuito limitado a 3 usuarios y 100 dispositivos
+ <a href="https://tailscale.com/download" target="_blank">Links de descarga</a>

# Flujo en general

## Obligatorio
1. Inicia sesión en TailScale
1. Une a tus primeros clientes

## Opcionales
1. Sección DNS
    1. Puede añadir un `Tailnet DNS name` custom (random)
    1. MagicDNS, te permite acceder a tus dispositivos a través de su hostname (Sección Machine)
    1. HTTPS Certificates

# Instalación en Debian

## Añadir repositorios e instala ZeroTier
~~~bash
curl -fsSL https://tailscale.com/install.sh | sh
~~~

## Entrar a la red
~~~bash
sudo tailscale up
~~~

Visita la URL que te indica la respuesta e inicia sesión con tu cuenta
~~~bash
To authenticate, visit:

	https://login.tailscale.com/a/id_open_your_id

Success.
~~~

## Corroborar status
~~~bash
tailscale status
~~~

## Reiniciar servicio (en caso de que lo requieras)
~~~bash
sudo systemctl restart tailscaled
~~~

## Prender o apagar

~~~bash
sudo tailscale down   # Apaga la VPN
sudo tailscale up     # Enciende la VPN
~~~

# Desinstalar TailScale

~~~bash
sudo systemctl stop tailscaled
sudo systemctl disable tailscaled

sudo apt remove tailscale

sudo rm -rf /var/lib/tailscale  # Aquí se guardan las llaves de la red
~~~

# Tailscale Funnel
Con Tailscale Funnel puedes redireccionar puertos expuestos a internet a puertos locales

## Abrir el puerto
~~~bash
sudo tailscale funnel 3000
~~~

## Corroborar servicios
~~~bash
tailscale serve status
~~~

## Cerrar el Funnel (Internet Público)
~~~bash
sudo tailscale funnel --off 5000
~~~

## Dejar de "Servir" el puerto (Privado y Público)
~~~bash
sudo tailscale serve --status
sudo tailscale serve 5000 off
~~~

## Limpieza total de configuraciones
~~~bash
sudo tailscale serve reset
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