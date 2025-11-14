---
title: Drozer
published: 2025-11-13
description: 'Pruebas de seguridad en Android con Drozer'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/drozer/portada.png'
tags: [Hacking, Android]
category: 'Hacking'
draft: false 
lang: 'es'
---

# ¿Qué es Drozer?
Drozer es un framework de código abierto que cuenta con una lista de módulos los cuales nos permiten hacer distintas pruebas de seguridad en Android

# Instalación
Para instalarlo se requieren dos cosas:
1. Drozer agent (aplicación apk)
1. Drozer console (instalado en la computadora)

## 1) Instalación de Drozer console
Existen otros métodos de instalación, estos los puedes encontrar en el repositorio oficial.
Para mí, los más sencillos son los siguientes.

### Pipx 
~~~bash
pipx install drozer
~~~

### Docker
~~~bash
sudo docker run --net host -it drozerdocker/drozer console connect --server _ip_
~~~

## 2) Instalación de Drozer agent
Drozer agent es la aplicación móvil que debes instalar en el celular
1. Descargas el apk desde <a href="https://github.com/ReversecLabs/drozer-agent" target="_blank">Github drozer agent</a>
1. Lo instalas, por ejemplo, mediante adb
~~~bash
adb install drozer-agent.apk
~~~

# Uso de Drozer
1. Abres la aplicación en el móvil y habilitas la opción "Embedded Server"
    + La aplicación debe estar abierta y en primer plano para funcionar
1. Habilitas el forward
~~~bash
adb forward tcp:31415 tcp:31415
~~~
1. Te conectas mediante la consola
~~~bash
# Si se instaló con pipx
drozer console connect --server _ip_

# Si se instaló con Docker
docker run --net host -it drozerdocker/drozer console connect --server _ip_
~~~

## Comandos

~~~bash
# Iniciando el Servidor
adb forward tcp:31415 tcp:31415

# Start the app in cellphone

# Connect to drozer
drozer console connect

# List apps and filtrate 
run app.package.list -f _name_

# Get info app
run app.package.info -a _name_

# View attack surface
run app.package.attacksurface _name_

# List activities
run app.activity.info -a _name_

# Start activities
run app.activity.start --component _app_name_ _activity_name_

# Scan provider URLs
run scanner.provider.finduris -a _app_name_

## Inside of a shell adb shell
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes"  

## Filter by a title of element
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection title

## using sql (manual)
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection "* FROM sqlite_master; --"

# provider query
run app.provider.query "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection "* FROM sqlite_master; --"

# Automated scan
run scanner.provider.injection -a _app_name_

# Automated show tables
run scanner.provider.sqltables -a _app_name_

## Inside of a shell adb shell
## Get a verion of sqlite
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection "sqlite_version() --" 

# List broadcast recivers
run app.broadcast.info -a _package_name_ -i

# Send broadcast recivers
run app.broadcast.send --action _name_ --extra string _var_name_ _value_ --extra string _var_name_ _value_
~~~

# Links
## Documentación oficial
+ <a href="https://github.com/ReversecLabs/drozer" target="_blank">Github drozer console</a>
+ <a href="https://github.com/ReversecLabs/drozer-agent" target="_blank">Github drozer agent</a>
+ <a href="https://labs.reversec.com/tools/drozer" target="_blank">Publicación</a>

## No oficial
+ <a href="https://exploitables.es/foro/index.php?threads/119/" target="_blank">Publicación no oficial que podría ser interesante</a>

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