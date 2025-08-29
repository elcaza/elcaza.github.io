---
title: Crea tu propio servidor al estilo Google Photos con Immich
published: 2025-08-23
description: 'En esta publicación se abordará lo relacionado a la instación y configuración de Immich para diversos casos de uso'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/immich/immich.png'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# ¿Qué es immich?

Un proyecto open source que te ayuda a montar un sistema como Google Photos pero en tu propio servidor.
Conserva muchas de sus facilidades como:
+ Cuentas independientes
+ Reconocimiento de rostros
+ Carpetas privadas
+ Carpetas compartidas
+ Aplicación móvil en Android e iOS

Y todo esto sin límite de almacenamiento (Todo dependerá de tu capacidad en disco duro). 

# Requisitos del sistema
+ Docker instalado
+ Al menos 4GB de ram y un procesador de 2 núcleos

# Guia de inicio
+ <a href="https://immich.app/docs/overview/quick-start" target="_blank">Quick start</a>


## Pasos generales de instalación:
1. Descarga los archivos requeridos
    + Selecciona el lugar donde vivirá la aplicación. Por ejemplo:
        + `/opt/immich`
    + Descarga `docker-compose.yml`
    + Descarga `.env`
2. Ingresa los valores requeridos del archivo .env *(Recuerda cambiar los datos por defecto)*
    + `UPLOAD_LOCATION=./library`. En caso de Raspberry Pi, un disco duro externo
    + `DB_DATA_LOCATION=./postgres`
    + `IMMICH_VERSION=release`
    + `DB_PASSWORD=postgres`
    + `DB_USERNAME=postgres`
    + `DB_DATABASE_NAME=immich`
3. Start the containers
	+ `sudo docker compose up -d`
4. Visitas la aplicación y completas el formulario
   + `http://<machine-ip-address>:2283`

# Bibliotecas externas
+ <a href="https://immich.app/docs/guides/external-library/" target="_blank">External library/</a>


Una biblioteca externa sirve para cargar tus fotografías sin necesidad de usar la interfaz web o aplicación móvil para subirlas.

Este proceso se realiza a través de dos apartados:
1. Configuración en Docker compose 
    + `docker-compose.yml`
1. Configuración en la aplicación
    + Administration => External Libraries Tab => Create Library => Edit import path

## Configuración en Docker compose 

1. Desde el lugar dónde instalaste tu immich detienes y eliminas los contenedores, redes y volúmenes definidos en tu archivo docker-compose.yml.
~~~bash
# Detenemos y eliminamos el contenedor actual
sudo docker compose down

# Editas el archivo docker-compose.yml
vim docker-compose.yml
~~~

2. Añades los volumenes a tu archivo de configuración
    + `/mnt/your/location/`**:**`/mnt/custom/docker/location`

Ejemplo:
~~~yml
immich-server:
    ...
    volumes:
      # Do not edit the next line. If you want to change the media storage location on your system, edit the value of UPLOAD_LOCATION in the .env file
      - ${UPLOAD_LOCATION}:/data
      - /etc/localtime:/etc/localtime:ro
      - /mnt/your/location:/mnt/custom/docker/location
      - /mnt/your/location/2:/mnt/custom/docker/location/2
~~~

3. Levantas nuevamente el contenedor
~~~bash
sudo docker compose up -d
~~~

## Configuración en la aplicación

Con la cuenta del administrador:
+ Administration 
+ External Libraries Tab 
+ Create Library 
+ Edit import path

## Referencias 
+ <a href="https://www.youtube.com/watch?v=gKgcGloFvgE" target="_blank">Vídeo en Youtube</a>
+ <a href="https://immich.app/docs/features/libraries/" target="_blank">Libraries/</a>
+ <a href="https://immich.app/docs/guides/external-library/" target="_blank">Eexternal library/</a>

# Respaldos
+ <a href="https://immich.app/docs/administration/backup-and-restore" target="_blank">Backup and restore</a>


## Creación de un respaldo

~~~bash
DB_USERNAME=username_database
sudo docker exec -t immich_postgres pg_dumpall --clean --if-exists --username=$DB_USERNAME | gzip > "./backup_$(date '+%Y-%m-%d_%H%M%S').sql.gz"
~~~

## Recuperación desde un respaldo

~~~bash
# Borra el contenedor actual
sudo docker compose down -v 

# Borra la ubicación de tu instalación de postgres para Immich
sudo rm -rf DB_DATA_LOCATION 

# Opcional - Update to latest version of Immich (if desired)
# sudo docker compose pull 

# Crea los contenedores de Docker sin ejecutarlos
sudo docker compose create

# Inicia el servidor de Postgres y espera 10 segundos a que inicie
sudo docker start immich_postgres && sleep 10                        

# Restaura los datos desde el Backup
PATH_BACKUP=/backup/location/dump_file.sql.gz
DB_NAME=db_name
DB_USERNAME=db_username

gunzip --stdout $PATH_BACKUP \
| sed "s/SELECT pg_catalog.set_config('search_path', '', false);/SELECT pg_catalog.set_config('search_path', 'public, pg_catalog', true);/g" \
| sudo docker exec -i immich_postgres psql --dbname=$DB_NAME --username=$DB_USERNAME  # Restore Backup

# Inicias de nuevo el contenedor
sudo docker compose up -d 
~~~

# Montado de discos externos con Ext4 con protección LUKS

Es posible que quieras utilizar un disco duro externo para guardar las fotos de tu servidor Immich y es sumamente probable que quieras proteger estas fotos ante un robo del disco duro físico. Para esto puedes utilizar un disco duro `Ext4` con protección `LUKS`. Con lo que tu información estará cifrada.

## Montado del disco
~~~bash
# Lista los dispositivos de bloque
lsblk

# Desciframos la información contenida en el disco
sudo cryptsetup luksOpen /dev/sdXN fotos

sudo mount /dev/mapper/fotos /mnt/ubicacion_para_montar
~~~
+ X = Letra
+ N = Número
+ fotos = cualquier alias

## Desmontado del disco

~~~bash
# Paramos nuestra instancia de immich (pues usa el disco)
sudo docker compose down

# Desmontado del disco y cifrado del mismo
sudo umount /mnt/fotos && sudo cryptsetup luksClose fotos
~~~

# Preguntas frecuentes

## ¿Cómo recuperar mi password?
+ <a href="https://immich.app/docs/administration/server-commands" target="_blank">server commands</a>

~~~bash
# Entras al contenedor de forma interactiva
sudo docker exec -it immich_server bash

# Ejecutas la utilidad para recuperar el password del administrador
immich-admin reset-admin-password

# Sales del contenedor
exit
~~~

## ¿Cómo desinstalar immich?
+ <a href="https://immich.app/docs/FAQ/#how-can-i-purge-data-from-immich" target="_blank">how can i purge data from immich</a>


1. Desde el lugar dónde instalaste tu immich detienes y eliminas los contenedores, redes y volúmenes definidos en tu archivo docker-compose.yml.

~~~bash
docker compose down -v
~~~

2. Borras todo lo relacionado a immich (se pueden consultar los valores en el archivo `.env`)
+ `DB_DATA_LOCATION` contains the database, media info, and settings.
+ `UPLOAD_LOCATION` contains all the media uploaded to Immich.

# Referencias
+ Sitio oficial https://immich.app/
+ Repositorio de Github

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