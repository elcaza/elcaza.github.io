---
title: Extraer IPA en iOS
published: 2026-09-08
description: 'Extraer IPAs en iOS'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/ios_extraer_ipa/portada.jpeg'
tags: [Hacking, Mobile, iOS]
category: 'Hacking'
draft: false 
lang: 'es'
---

# 1. Encontrar aplicaciones dentro de iOS a través de una conexión ssh

## Forma manual
~~~bash
# Define IP de tu iPhone
IP=10.1.1.1

# Inicia
ssh root@$IP

# Listar todas las aplicaciones - automatizado
find /var/containers/Bundle/Application/ -iname "*.app" | cut -d "/" -f7 | sort 

# Define la aplicación a extraer
APPNAME="NAME"
~~~

## Opción 1: Extracción de IPA manual

~~~bash
# Entras al directorio de aplicaciones
cd /var/containers/Bundle/Application/

# Encuentras el UUID Folder de tu aplicación
find | grep $APPNAME | cut -d "/" -f2 | sort -u 

# Entras UUID-Folder
cd <UUID_Folder>

# Creas una carpeta para almacenar tu app
mkdir Payload

# Copia la app a la carpeta temporal (Solo debería existir un archivo .app)
cp -r  *.app/ Payload/

# Crear tu archivo .ipa
zip -r /var/root/$APPNAME.ipa Payload/

# Identifica el IPA a descargar
echo "get -r $(ls /var/root/$APPNAME.ipa) ."

# Descargas la aplicación
sftp root@$IP
get -r /var/root/<NAME_TO_EXTRACT_IPA>.ipa
~~~

Notas:
~~~txt
Why “Payload”? The name ‘Payload’ is part of the standard iOS app bundle structure required for the iOS operating system to correctly recognize and install the app. In the .ipa file format, the app bundle must be placed inside a ‘Payload’ folder containing the .app directory. Failure to follow this structure may prevent the app from being recognized or executed.

Capital letter is important: "Payload" not "payload"

~~~
Fuente:
+ https://awesome.hackpuntes.com/ios/how-to-extract-ipa-from-ios-device
+ https://medium.com/haktrak-cybersecurity-squad/bypassing-ios-jailbreak-detection-by-patching-the-binary-with-ghidra-write-up-of-no-escape-lab-02b417b3a235

## Opción 2: Extracción de IPA automatizada

### Requisitos previos

~~~bash
# Se instala zip
apt install zip
~~~

### Definir APPNAME

+ Este script se debe correr con `bash`, no con `zsh`
~~~bash
# Script Automatizado, reemplaza el valor de APPNAME
cd /var/containers/Bundle/Application/; if [ $(find | grep $APPNAME | cut -d "/" -f2 | sort -u | wc -l) == "1" ]; then echo "Comenzando respaldo" && UUID_Folder=$(find | grep $APPNAME | cut -d "/" -f2 | sort -u); cd $UUID_Folder; rm -rf Payload; mkdir Payload; ls | grep .app; NAME_TO_EXTRACT_IPA=$(ls | grep .app); cp -r $NAME_TO_EXTRACT_IPA Payload/; IPA_FINAL=$(printf '%s' $(cut -d. -f1 <<< $NAME_TO_EXTRACT_IPA)).ipa; zip -r /var/root/$IPA_FINAL Payload && echo "Guardado como:"; ls /var/root/ | grep "ipa"; echo -e "\nListo \n\nPara descargar:\n"; echo "scp root@IP:/var/root/$IPA_FINAL ."; sleep 5; else echo "Too many apps with that name"; fi;
~~~
Para descargar el IPA se te mostrará el comando que debes introducir. Similar a
`scp root@IP:/var/root/$IPA_FINAL .`

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