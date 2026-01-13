---
title: Borgbackup
published: 2026-01-05
description: 'Crea respaldos deduplicados y con control de versiones con BorgBackup'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/borgbackup/portada.jpg'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# ¿Qué es?
Borg Backup es una herramienta que te permite:
+ Generar respaldos (estos se almacenan cifrados)
+ Llevar un control de versiones (muy útil ante perdida de datos)
+ Deduplicado, únicamente se almacenan los nuevos datos
+ Puedes almacenar tu respaldo en otro servidor

# Enlaces
+ <a href="https://www.borgbackup.org/" target="_blank">Sitio web oficial</a>
+ <a href="https://github.com/borgbackup/community" target="_blank">Github</a>

# Instala Borg en ambos equipos
~~~bash
sudo apt install borgbackup -y
~~~

# Configura SSH sin contraseña
Desde tu equipo que hará el respaldo
~~~bash
ssh-keygen -t ed25519

ssh-copy-id usuario_debian@ip_del_servidor
~~~

# Inicializa el repositorio de respaldo
~~~bash
borg init --encryption=repokey usuario_debian@ip_del_servidor:/home/usuario_debian/backups/immich_nextcloud
~~~

# Script

~~~bash
#!/bin/bash

# Configuración
REPOSITORY="usuario_debian@ip_del_servidor:/home/usuario_debian/backups/immich_nextcloud"
export BORG_PASSPHRASE="tu_contrasena_de_borg"

# 1. Pausar los contenedores (Opcional, pero recomendado para consistencia total)
# O puedes usar comandos específicos de dump para Postgres/MariaDB si prefieres no detenerlos
docker stop immich_server nextcloud_db_1 

echo "Iniciando respaldo..."

# 2. Crear el respaldo de los volúmenes de Docker
borg create --stats --progress \
    $REPOSITORY::"backup-$(date +%Y-%m-%d_%H-%M)" \
    /ruta/a/tus/volumenes/docker/immich \
    /ruta/a/tus/volumenes/docker/nextcloud

# 3. Reanudar contenedores
docker start immich_server nextcloud_db_1

# 4. Limpieza (Prune) para mantener solo lo necesario
borg prune -v $REPOSITORY --keep-daily=7 --keep-weekly=4 --keep-monthly=6

echo "Respaldo completado con éxito."
~~~

~~~bash
borg create -s -p \
user@server:/mnt/disk/server::backup_{now:%Y-%m-%d_%H:%M} \
/mnt/server
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