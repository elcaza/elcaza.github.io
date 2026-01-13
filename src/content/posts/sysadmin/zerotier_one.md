---
title: ZeroTier One
published: 2025-12-16
description: 'Conecta tus dispositivos a través de una VPN con ZeroTier One'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/zerotier_one/portada.png'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://www.zerotier.com/" target="_blank">Sitio web oficial</a>
+ <a href="https://central.zerotier.com/" target="_blank">Panel de inicio de sesión</a>
+ <a href="https://docs.zerotier.com/quickstart/" target="_blank">Documentación</a>

# Instalación en Debian

## Añadir repositorios e instala ZeroTier
~~~bash
curl -s 'https://raw.githubusercontent.com/zerotier/ZeroTierOne/main/doc/contact%40zerotier.com.gpg' | gpg --import && if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi
~~~

## Entrar a la red
~~~bash
sudo zerotier-cli join YOUR_NETWORK_ID
~~~

## Reiniciar servicio (en caso de que lo requieras)
~~~bash
systemctl restart zerotier-one
~~~

# Descarga los clientes
+ <a href="https://www.zerotier.com/download/" target="_blank">Clientes</a>

# ¿Cómo desinstalar?

~~~bash
sudo systemctl stop zerotier-one
sudo apt remove --purge zerotier-one
sudo rm -rf /var/lib/zerotier-one
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