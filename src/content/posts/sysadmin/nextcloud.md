---
title: NextCloud con Docker
published: 2025-12-21
description: 'Crea tu propia nube virtual, al estilo Google Drive con NextCloud'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/nextcloud/portada.webp'
tags: [Sysadmin, Contenedores]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://nextcloud.com/install/" target="_blank">Sitio web oficial</a>
+ <a href="https://docs.nextcloud.com/server/latest/admin_manual/installation/" target="_blank">Manual</a>
+ <a href="https://github.com/nextcloud/all-in-one#nextcloud-all-in-one" target="_blank">Docker</a>

# Uso en Docker

## Creación de los siguientes archivos

### Env

`.env`

~~~bash
# DB
MYSQL_ROOT_PASSWORD=your_root_password
MYSQL_PASSWORD=your_user_password
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextcloud

# Rutas
NEXTCLOUD_DB_DIR=./db
NEXTCLOUD_DATA_DIR=./data
NEXTCLOUD_CONFIG_DIR=./config
~~~

### Yaml

`docker-compose.yml`

~~~yml
services:
  db:
    image: mariadb:10.6
    container_name: nextcloud-db
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - ${NEXTCLOUD_DB_DIR}:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}

  redis:
    image: redis:alpine
    container_name: nextcloud-redis
    restart: always

  app:
    image: nextcloud:latest
    container_name: nextcloud-app
    restart: always
    ports:
      - 8080:80
    depends_on:
      - db
      - redis
    volumes:
      - ${NEXTCLOUD_DATA_DIR}:/var/www/html/data
      - ${NEXTCLOUD_CONFIG_DIR}:/var/www/html/config
    environment:
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_HOST=db
      - REDIS_HOST=redis

~~~

## Creación de las carpetas requeridas para el sistema

~~~bash
# Crear carpetas de sistema (donde está el .yml)
mkdir -p ./config ./db ./data

# Aplicar permisos del usuario www-data (ID 33 en Docker)
sudo chown -R 33:33 ./config ./db ./data

# Docker
sudo docker exec -u 33 nextcloud-app php occ files:scan --all
~~~

## Inicio del contenedor

~~~bash
sudo docker compose up -d
~~~

# En caso de migrar el servidor 

Podrías obtener el siguiente mensaje de error: "Access through untrusted domain".
Para solucionarlo:
1. Abre el siguiente archivo: `config/config.php`
1. Modifica lo siguiente

~~~bash
'trusted_domains' => 
  array (
    0 => 'your_domain:8080',
  ),
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