---
title: Notas sobre hacking Android
published: 2025-08-11
description: 'Description'
image: ''
tags: [Apuntes]
category: 'Apuntes'
draft: false 
lang: 'es'
---

# Android pentest

************************************************************************************************
# Índice
1. Teoría
1. ADB
1. Android Tools

************************************************************************************************
# Teoría

## Rooteando Android
+ Otorgarle privilegios de root a tu celular requiere un celular físico o virtual

### En caso de celualar físico 
1. Nota: Cada dispositivo, fabricante y versión de S.O. tiene su propio método, pero a grandes rasgos se debe hacer lo siguiente
1. Instalar los drivers de celular
1. Desbloquar bootloader
1. Instalar TWRP (o equivalente) e inicializarlo
1. Instalar Magisk o equivalente (KernelSU, Kitsune, étc)
1. Corroborar estado de rooteo con alguna aplicación (Root Checker Basic, RootBeer Ample, Integrity Checker, YASNAC)
1. Instalar certificado de PortSwigger
1. Instalar plugins/módulos de magisk
	+ Always Trust User Certificates (Jeroen Beckers) => Confía en el certificado de Burp y puedes leer el tráfico cifrado
	+ Play Integrity Fix (chiteroman) => Evade la detección de un par de validaciones anti root
	+ Shamiko (LSPosed Developers) => Evade la detección de un par de validaciones anti root
	+ Zygisk - LSPosed => Evade la detección de un par de validaciones anti root
1. En teoría ahora tienes lo básico en tu celular para hacer pruebas
+ En caso de que el dispositivo sea virtual
1. Se puede usar GenyMotion
	+ Está rooteado por defecto, solo debes instalar las aplicaciones correspondientes
1. Android Emulator (Android Studio)
	+ Se puede rootear cuando este no tiene Google Play Services (Al momento de crearse se selecciona sin el icono de PlayStore)

### En caso de celualar virtual (Genymotion) 
+ Pendiente
	+ https://www.genymotion.com/blog/tutorial/magisk-genymotion/#elementor-toc__heading-anchor-1

## Herramientas utiles

### Virtualizadores:
+ Genymotion
	+ Virtualizador
+ Android Studio
	+ Virtualizador

### Análisis de código
+ Mobile-Security-Framework-MobSF
	+ All-in-one mobile application (Android/iOS/Windows)
	+ Análisis automatizado de la aplicación
+ apktool
	+ Decompilar la aplicación
	+ Puedes hacer modificaciones de código en el Smali
	+ Compilar la aplicación
	+ Nota: Se debe firmar la aplicación con jarsigner
+ jadx
	+ Decompilar la aplicación. Tiene el modo --deobf para desofuscar el código

### Pruebas dinámicas
+ drozer
	+ Drozer es una herramienta que cuenta con una lista de módulos los cuales nos permiten hacer distintas pruebas de seguridad
+ adb
	+ Android Debug Bridge, para conectarse al teléfono
+ SSHDroid
	+ Servidor SSH
+ Frida
+ Objection

### Utilidades
+ Netcat
	+ Herramienta de red

## Locations with information
~~~bash
# System apps are located in
/system/app

# User installed apps are located in (here you can find app.apk)
/data/app/ 

# Data of apps
/data/data/

## Shared preferences (if an app use a insecure data storage)
/data/data/app/shared_prefs/

# It may contains a embebed strings with 
app/res/values/strings.xml

~~~

## Genymotion info
~~~
### You can now use these tools from [/opt/genymobile/genymotion]:
- genymotion
- genymotion-shell
- gmtool

### /opt/genymobile/genymotion/tools
~~~

## Puntos importantes
1. Tras decompilar con apktool
	+ Android Manifest tiene el nombre de las actividades y algunos servicios. A su vez permisos para ejecución
		+ Activity MAIN ACTIVITY muestra la actividad que ejecutará al inicializar la aplicación
	+ app/res/values/strings.xml puede tener strings como passwords, API Keys, información interesante
		+ google_api_key
		+ google_api_id
		+ project_id
			+ If firebase is on debug name you can check all data of your database with something similar to (need to get it with petition intercept)
				+ curl -X GET "https://firestore.googleapis.com/v1/projects/name_of_project/(default)/documents/Tweets/"
				+ https://firebase.google.com/docs/firestore/use-rest-api?hl=es-419
1. ProGuard is a thing inside of Android Studio to Obfuscate and do small your code
1. AndroidManifest.xml => allowBackup="true" puede ser inseguro
1. You can manipulate an apk for allow trusth in users certificates 


************************************************************************************************
# Basics ADB

## Conexión

### Basics ADB

Puertos default:
+ 5000 (adb)

Comandos Básicos
~~~bash
# Mostrar menú de la herramienta
adb

# ADB as root
adb shell
su

# Conectarte a un dispositivo mediante adb
adb connect 172.27.253.6:5555

# Listar dispositivos
adb devices

# Instalar en aplicación en emulador
adb -s <emulator> install <application.apk>

# Reemplaza la copia existente de la aplicación
adb -s <emulator> install -r <application.apk>

# Ingresa a una shell de un device
adb -s <emulator> shell

# Envía un documento a un emulador
adb -s <emulator> push '/home/user/local/file' '/data/local/' 

# Descarga un documento desde un emulador
adb -s pull 'sdcard/log.txt' '/home/user/local/file'

# Listar apps instaladas
adb shell cmd package list packages

# Start activity on device
am start -n com.android.package/.activity_name
## am start -n com.android.insecurebankv2/.PostLogin  
~~~

### ADB on the network (unencrypted traffic)
~~~bash
# Using usb cable and adb get the ip cellphone
adb shell ip -f inet addr show wlan0
adb shell "ip addr show wlan0 | awk '\$1 == \"inet\" {print \$2}' | cut -f1 -d/"

# setting up adb over wlan
adb tcpip 5555

# checking service up
adb shell netstat -pnta | grep 5555
## look LISTEN service

# Exit and disconnect USB cable

# Connect
adb connect IP:PORT (192.168.100.10:5555)
## Return connected to...

# For disconnect 
adb disconnect 

### Note: The traffict is unencrypted   

# For closed service on port over WLAN
adb usb

~~~

### ADB útiles
~~~bash
# Ver logs
abd logcat

# Create tcpdump in mobile
tcpdump -v -s 0 -w my_pcap.pcap
~~~

### Basics pm
~~~bash
# Listar paquetes instalados
pm list packages

# Ver el path de la app
pm path com.android.name

~~~

************************************************************************************************
# Android Tools

## apktool
~~~bash
# Decompile the app
apktool d app.apk

# The code smali can be modificated
http://pallergabor.uw.hu/androidblog/dalvik_opcodes.html

# Doing a patched app
apktool b folder -o patched_app.apk

# Sing the new app
## Check for "Firmar aplicaciones"

~~~
+ https://apktool.org/docs/install

## Firmar aplicaciones
~~~bash
# Generate a key to firm the app
keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000

# Sing the app
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.keystore your_app.apk alias_name
	# Install
	# sudo apt install default-jdk-headless 
~~~

## jadx
~~~bash
# Decompile
jadx -d new_folder app.apk

# If the apk is obfuscated
## It's better than MobSF to decompile obfuscated code
jadx -d new_folder app.apk --deobf

~~~

Repositorio
+ https://github.com/skylot/jadx/tree/v1.5.1
+ https://github.com/skylot/jadx/releases/tag/v1.5.1
+ https://lindevs.com/install-jadx-on-ubuntu

## Drozer 
~~~bash
# Iniciando el Servidor
adb forward tcp:31415 tcp:31415

# Start the app in cellphone

# Connect to drozer
drozer console connect

# List apps and filtrate 
run app.package.list -f <name>

# Get info app
run app.package.info -a <name>

# View attack surface
run app.package.attacksurface <name>

# List activities
run app.activity.info -a <name>

# Start activities
run app.activity.start --component <app_name> <activity_name>

# Scan provider URLs
run scanner.provider.finduris -a <app_name>

## Inside of a shell adb shell
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes"  

## Filter by a title of element
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection title

## using sql (manual)
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection "* FROM sqlite_master; --"

# provider query
run app.provider.query "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection "* FROM sqlite_master; --"

# Automated scan
run scanner.provider.injection -a <app_name>

# Automated show tables
run scanner.provider.sqltables -a <app_name>

## Inside of a shell adb shell
## Get a verion of sqlite
content query --uri "content://jakhar.aseem.diva.provider.notesprovider/notes" --projection "sqlite_version() --" 

# List broadcast recivers
run app.broadcast.info -a <package_name> -i

# Send broadcast recivers
run app.broadcast.send --action <name> --extra string <var_name> <value> --extra string <var_name> <value>
~~~
More info:
+ https://github.com/WithSecureLabs/drozer-agent
+ https://labs.withsecure.com/tools/drozer


## Mobile-Security-Framework-MobSF
By Docker install
~~~bash
# To run for first time
sudo docker run -it --name mobsf -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest

# Visit your localhost or IP on 8000 port
localhost:8000
## Default user and password
mobsf:mobsf

# To stop container
sudo docker stop mobsf

# To start again the container
sudo docker start mobsf

# More info

## To lists all containers
sudo docker ps -a

## To lists active containers
sudo docker ps

## To remove container
docker rm <id_contenedor>

## To remove container image
sudo docker rmi <id_imagen>
~~~

## Dex2jar

https://github.com/pxb1988/dex2jar

## Sign
https://github.com/appium-boneyard/sign


## sqlite3
~~~bash
sqlite3 <db>

# List tables
.tables


# Select
SELECT * FROM <table>

~~~

## Objection
+ https://medium.com/@ria.banerjee005/bypassing-ssl-pinning-with-frida-and-objection-in-mobile-applications-0b42a778b0f2
+ https://petruknisme.medium.com/easy-way-to-bypass-ssl-pinning-with-objection-frida-beginner-friendly-58118be4094

### Dependencias
~~~bash
sudo apt install apksigner
sudo apt install zipalign.
~~~

## Frida

Frida requiere dos aplicaciones: 
1. Frida server (instalado en el celular) 
1. Frida client (Instalado en la computadora)
+ Nota: Ambos deben tener la misma versión instalada. Por ejemplo: 16.7.13

Instalar cliente de Frida en Debian 12
~~~bash
# Instalar pipx
sudo apt install pipx

# Instalar frida-tools
pipx install frida-tools

# Instalar una versión en especifica
pipx install frida-tools==16.7.13

# Solamente si no lo detecta el PATH
pipx ensurepath
	# Note: '/home/user/.local/bin' is not on your PATH environment variable. These apps will not be globally accessible until your PATH is updated. Run `pipx ensurepath` to automatically add it, or manually

# Obtener la versión de Frida
 frida --version
	# 16.7.13

# Actualizar Frida
pipx upgrade frida-tools	
	# 17.2.11

# Quitar Frida
pipx uninstall frida-tools
~~~

Desde la computadora - Descargar Frida Server para el celular (debe coincidir con la que tienes instalada)
+ https://github.com/frida/frida
	+ Releases
	+ Si tu celular tiene arquitectura de 64 bits (x64)
		+ Versión Frida-Server Android ARM64 (frida-server-16.7.13-android-arm64.xz)
		+ https://github.com/frida/frida/releases/download/16.7.13/frida-server-16.7.13-android-arm64.xz
	+ Si tu celular tiene arquitectura de 32 bits (x86)
		+ Versión Frida-Server Android ARM (frida-server-16.7.13-android-arm.xz)
		+ https://github.com/frida/frida/releases/download/16.7.13/frida-server-16.7.13-android-arm.xz

Descomprimir y cargar en el celular frida-server (Desde la computadora)
~~~bash
# Descomprimir frida server
xz -d frida-server-16.7.13-android-arm64.xz 

# Cambiar el nombre
mv frida-server-16.7.13-android-arm64 frida-server

# Subir frida-server al celular
adb push frida-server /data/local/tmp

# Inicializar frida-server
adb shell
su
cd /data/local/tmp/
chmod +x frida-server
./frida-server &

# Verificar que frida-server este corriendo
ps | grep frida
	# root          8828 15283   49124  22764 poll_schedule_timeout 0 S frida-server


~~~

## Objection
Instalar Objection
+ Es requerido contar con Frida para hacer uso de este módulo

~~~bash
# Instalar pipx
sudo apt install pipx

# Instalar Objection
pipx install objection

# Probar Objection
objection --gadget "com.android.settings" device-type
	# Using USB device `moto g 7  play`
	# Agent injected and responds ok!
	# Connection: USB
	# Name: com.android.settings
	# System: channel
	# Model: motorola
	# Version: 10
	# Asking jobs to stop...
	# Unloading objection agent...
~~~

# Free labs
+ https://github.com/atilsamancioglu/MS2-WordGame
	+ Change the function to gain score with anything word
+ https://github.com/dineshshetty/Android-InsecureBankv2
	+ Insecure Bank app
+ https://github.com/atilsamancioglu/MS1-SecureTweet
	+ Needed a course to explain how to compile
+ https://github.com/httptoolkit/android-ssl-pinning-demo
	+ SSL Pinning

************************************************************************************************
# Bypassing SSL Pinning

## Frida Scripts

### Comentario
+ Al correr los scripts hay una diferencia entre la opción `-f` y `-F`
	+ `-f` TARGET, --file TARGET spawn FILE
	+ `-F`, --attach-frontmost

~~~bash
# Con -f indicas el binario a abrir para hacerle ssl pinning
# Este no siempre funciona, puede tronar la app
frida --codeshare akabe1/frida-multiple-unpinning -f YOUR_BINARY

# Con -F agarra la app que tengas abierta
# Con este no se obtuvieron errores
frida --codeshare akabe1/frida-multiple-unpinning -F
~~~

### akabe1 - frida-multiple-unpinning
Link
+ https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/

Notas:
+ Funcionó en determinada aplicación. Muy efectivo.

~~~bash
# Obtener nombre de la aplicación
frida-ps -Uia | grep app_name
	# 29233	My app	com.app.name

# From file
frida --device 192.168.1.1:5555 -f com.app.name -l frida-script.js 

# From codeshare
frida --codeshare akabe1/frida-multiple-unpinning --device 192.168.1.1:5555 -f com.app.name

# Si solo se tiene un dispositivo vinculado, puede hacerse de la siguiente manera
frida --codeshare akabe1/frida-multiple-unpinning -f YOUR_BINARY
frida --codeshare akabe1/frida-multiple-unpinning -U -f com.app.name
~~~

## Otros scripts de Frida:
~~~bash
frida --codeshare sowdust/universal-android-ssl-pinning-bypass-2 -U -f YOUR_BINARY
# No funcionó
frida --codeshare pcipolloni/universal-android-ssl-pinning-bypass-with-frida -U -f YOUR_BINARY
# No funcionó
frida --codeshare sowdust/universal-android-ssl-pinning-bypass-2 -U -f YOUR_BINARY
# No funcionó
frida --codeshare Q0120S/bypass-ssl-pinning -U -f YOUR_BINARY
# No funcionó, pero avanza más
frida --codeshare hyugogirubato/android-ssl-pinning -U -f YOUR_BINARY
# No funcionó, pero avanza más

# IOS
# frida --codeshare federicodotta/ios13-pinning-bypass -U -f YOUR_BINARY
# frida --codeshare federicodotta/ios13-pinning-bypass -U -f YOUR_BINARY
~~~

## Objection
+ No funcionó en determinada aplicación
	+ (agent) [390586] Called (Android 7+) TrustManagerImpl.checkTrustedRecursive(), not throwing an exception.

~~~bash
# Obtener nombre de la aplicación
frida-ps -Uia | grep app_name
	# 29233	My app	com.app.name

# Inicializar la app
objection --gadget "com.app.name" explore

# Deshabilitar ssl pinning
android sslpinning disable
~~~

************************************************************************************************
# Instalación adb y fastboot
~~~bash
sudo apt install fastboot adb
~~~

************************************************************************************************
# Anexo - Otros recursos

## Laboratorios de Android

Frida y Objection
+ https://petruknisme.medium.com/easy-way-to-bypass-ssl-pinning-with-objection-frida-beginner-friendly-58118be4094
+ https://github.com/frida/frida
+ https://github.com/sensepost/objection

## Magisk
+ https://github.com/topjohnwu/Magisk?tab=readme-ov-file