---
title: Fail2Ban
published: 2025-12-21
description: 'Banea a atacantes con Fail2Ban'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/fail2ban/portada.jpeg'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://github.com/fail2ban/fail2ban" target="_blank">Github</a>

# Instalar fail2ban e ipset
~~~bash
sudo apt install fail2ban ipset
~~~

# Configuraciones iniciales
~~~bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo vim /etc/fail2ban/jail.local
~~~


# Ingresa las siguiente configuraciones

Este es solo un ejemplo para el servicio ssh. Ajustar según las necesidades.
~~~bash
[INCLUDES]
before = paths-debian.conf

[DEFAULT]
# 1. SEGURIDAD BÁSICA
ignoreip = 127.0.0.1/8 ::1
enabled = false
findtime  = 1d
maxretry = 7
bantime  = 1d

# 2. PARÁMETROS DE FUNCIONAMIENTO
backend = auto
usedns = warn
logencoding = auto
mode = normal
filter = %(__name__)s[mode=%(mode)s]

# 3. ACCIÓN DE BANEO (Solo Firewall, sin rastro de email)
protocol = tcp
chain = <known/chain>
port = 0:65535

# Usamos ipset para máximo rendimiento
banaction = iptables-ipset-proto6
banaction_allports = iptables-allports

# Definimos que la acción sea estrictamente banear
action = %(banaction)s[port="%(port)s", protocol="%(protocol)s", chain="%(chain)s"]

# --- CÁRCELES (JAILS) ---

[sshd]
enabled = true
port    = ssh
backend = systemd
maxretry = 5

[recidive]
enabled = true
logpath  = /var/log/fail2ban.log
banaction = %(banaction_allports)s
bantime  = 1w
findtime = 3d
maxretry = 10
~~~

# Reiniciar fail2ban
~~~bash
sudo systemctl restart fail2ban
sudo systemctl status fail2ban
~~~

# Ver estatus del servicio sshd
~~~bash
sudo fail2ban-client status sshd
~~~

# Desbanear una IP
~~~bash
sudo fail2ban-client status sshd

sudo fail2ban-client set sshd unbanip _IP_
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