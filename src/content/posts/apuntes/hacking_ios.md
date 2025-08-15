---
title: Hacking iOS
published: 2025-08-11
description: 'Description'
image: ''
tags: [Apuntes]
category: 'Apuntes'
draft: false 
lang: 'es'
---

# Hacking iOS

************************************************************************************************
# Índice
1. Teoría
1. Jailbreak
1. Instalación de herramientas necesarias
	+ Iphone
	+ Computadora
1. Casos de uso
	+ Extracción del IPA
	+ Análisis estático de la aplicación
	+ Evasión de la detección anti-jailbreak
	+ Evasión de la protección SSL-Pinning

************************************************************************************************
# Teoría

## ¿Por qué la gente hace Jailbreak?
Las personas pueden optar por realizar el jailbreak de sus dispositivos para realizar acciones que no estén permitidas por Apple. Dos casos de uso pueden ser:
1. Realizar pruebas de seguridad informática
1. Agregar "Tweaks" (ajustes, personalizaciones) a su iPhone

## Tipos de Jailbreak
+ rootful
	+ Da acceso completo al sistema, también al sistema raíz "/". Te da privilegios de root
+ rootless
	+ No tienes acceso completo al sistema, no puedes acceder a raíz "/", pero sí a otros directorios. También tienes privilegios de root.

## Tipos de persistencia
+ Tethered
	+ 📱 🔄 📵 💻 ✅
	+ Requiere la conexión a una computadora cada vez que se reinica. Sin esto el teléfono no iniciará de manera correcta.
+ Untethered
	+ 📱 🔄 ✅
	+ El teléfono puede apagarse y reiniciarse sin perder funcionalidades ni su estado de jailbreak. Sin embargo, este tipo de jailbreak comenzó a desaparecer con iOS 9
+ Semi-tethered
	+ 📱 🔄 📱🔒 💻 ✅
	+ Si el teléfono se apaga o reinicia, el teléfono podrá seguir utilizandose pero perderá su estado de jailbreak. Este puede volver a activarse mediante el mismo proceso que le hizo jailbreak en un inicio.
	+ Actualmente, este es el jailbreak más utilizado.
+ Semi-Untethered Jailbreak
	+ 📱 🔄 📱🔒 📲 ✅
	+ Si el teléfono se apaga o reinicia, el teléfono podrá seguir utilizandose pero perderá su estado de jailbreak. A diferencia del "semi-tethered" este puede volver a activarse mediante una aplicación que devuelva el estado de jailbreak.


************************************************************************************************
# Requisitos para jailbreak 
+ Iphone con procesador inferior al A12 (Iphone 8 Plus y anteriores. Iphone X, pero solo el X, ninguna de sus variantes)
+ iOS soportados y probados de primera mano:
	+ iOS 15.6.1 probado con Iphone 8 - jailbreak con palera1n
		+ Usando live usb. Desde Debian y winra1n no funciono
		+ Usando un cable usb A desde una computadora con puertos USB 2.0
	+ iOS 14.7.1 probado con Iphone 7 - jailbreak con checkra1n 
		+ Debian app
		+ Usando un cable usb A desde una computadora con puertos USB 3.0
+ Notas generales
	+ Es un proceso que puede tomar un par de intentos
	+ Debes conocer cómo entrar al modo DFU
		+ Puedes ensayar viendo qué modo detecta iTunes (conectado el celular a la computadora)
	+ A través de un cables USB A es más fácil que se realice el proceso correctamente. USB C, puede fallar
	+ Es más fácil a través de los puertos USB 2.0 de la computadora

## Dispositivos probados y versión de iOS - ¿Cómo entrar al modo DFU?
+ iOS 15.6.1 probado con Iphone 8 - jailbreak con palera1n (live usb)
	+ Entrar al modo DFU 
		+ UP, DOWN, SIDE-hasta-apagar, SOLTAR_TODO, SIDE+DOWN-5segundos, DOWN-5segundos (o hasta que se comience el proceso)
		+ PRESION_RAPIDA, PRESION_RAPIDA, SOSTENER, SOLTAR_TODO, PRESIONAR_AMBOS_SOSTENER, SOLTAR_SOLO_SIDE_BUTTON
+ iOS 14.7.1 probado con Iphone 7 - jailbreak con checkra1n (Debian app)
	+ Entrar al modo DFU 
		+ SIDE+DOWN-10segundos, DOWN-10segundos (o hasta que se comience el proceso)
		+ PRESIONAR_AMBOS, SOLTAR_SOLO_SIDE_BUTTON

## Consideraciones generales para el Jailbreak
+ Otorgarle privilegios de root (Jailbreak) a tu celular requiere un celular físico 
+ Rootful
	1. Nota: Cada dispositivo y versión de S.O. tiene su propio método, pero a grandes rasgos se debe hacer lo siguiente
	1. Quitar FaceID, passcode, TouchID, desactivar Find Iphone * Vease en un tutorial propio de la herramienta utilizada
	1. Tener espacio disponible en el celuar (10GB) * Vease en un tutorial propio de la herramienta utilizada
	1. Usar palera1n o checkra1n (según sea el caso)
		+ Debes entrar al modo DFU * Vease en un tutorial propio del disposito
		+ Instalar Cydia, Sileo, Zebra * Vease en un tutorial propio de la herramienta utilizada


************************************************************************************************
# Palera1n
## Notas 
+ Requieres 10GB libres de almacenamiento
+ Dale a `Trust` cuando lo conectes a la computadora

## Proceso
+ Crear USB Booteable
+ Iniciar con el ISO
+ Seleccionar Palera1n
+ Si es primera vez configurar las siguientes opciones
	+ 1 Create FakeFS
	+ 3 Verbose
	+ 7 Debug
	+ Arguments: -f -c -V -v
+ Conectar tu celular y ponerlo en modo DFU
+ Dejar que trabaje y, una vez terminado podás notar que tienes 10Gb menos en tu almacenamiento
+ Iniciar con el ISO
+ Seleccionar Palera1n
+ Seleccionar las siguientes opciones
	+ 3 Verbose
	+ 7 Debug
	+ Arguments: -f -V -v
+ Conectar tu celular y ponerlo en modo DFU
+ Dejar que trabaje y, una vez terminado podás notar que tienes 10Gb menos en tu almacenamiento


## Problemas y soluciones
+ Reiniciar el programa
	+ `CTRL +C`
	+ `exit`


************************************************************************************************
# Herramientas para realizar pruebas de seguridad:

## Dependencias

### ElleKit - Instalar dependencias
+ Abrir Sileo
+ Ir a sources => Añadir
	+ `https://ellekit.space/` 
+ Ir a `Search` y buscar `ElleKit`
+ Instalar `ElleKit`
	+ En caso de que no instale presionar los botones: Get => Queque => Confirm => Done

## Administración de IPAs

### Servidor SSH - Instalar un servidor SSH
+ Abrir Sileo
+ Ir a `Search` y buscar `Openssh`
+ Instalar `openssh` y todas sus dependencias (4)
	+ En caso de que no instale presionar los botones: Get => Queque => Confirm => Done
+ Obtener la IP del celular (Settings => WIFI => <your_red> => IP Address)
+ `ssh mobile@192.168.x.x`
	+ El password es el que hayas preconfigurado al momento de rootear el celular

### Filza - Gestor de archivos
+ Abrir Sileo
+ Ir a sources => Añadir
	+ `https://tigisoftware.com/cydia/` 
+ Ir a `Search` y buscar `Filza`
+ Instalar `Filza File Manager 64-bit`
	+ En caso de que no instale presionar los botones: Get => Queque => Confirm => Done

### AppSync Unified - Instalar aplicaciones aunque no se puedan verificar sus firmas
+ Descargar AppSync para evitar el error "Failed to verify code signature of"
	+ https://github.com/akemin-dayo/AppSync/releases/download/116.0/ai.akemi.appsyncunified_116.0_iphoneos-arm.akemi-git-235aca6cddfbdc9fa87fcb5b2aec2df37ed6d65a.deb 
		+ iphoneos-arm is for rootrw/fakefs ("rootful") jailbreaks
		+ iphoneos-arm64 is for rootless jailbreaks
+ Abrir en "Files" => "Quick look" => "icono share" => "Open Filza" => "Seleccion de aplicación" => "Install"
+ Abrir la aplicación con Filza

## Intercepción de tráfico

### Certificado BurpSuite - Añadir certificado de BurpSuite para interceptar tráfico
+ Configura tu celular para pasar por el proxy de BurpSuite
+ Visita: http://burpsuite
+ Da click en `CA CERTIFICATE` => `Allow` => `Review the profile in Settings app if you want to install it`
+ Instala el certificado 
	+ `Settings` => `General` => `VPN & Device Managment` => `Downloaded profile | PortSwigger CA` => Install
+ Habilita la confianza de raíz en el certificado
	+ `Settings` => `General` => `About` => `Certificate Trust Settings` => Enable

### SSL Kill Switch 3 - Remover SSL Pinning
+ Descargar `SSL Kill Switch 3`
+ Abrir Sileo
+ Ir a `Search` y buscar `SSL Kill Switch 3`
+ Instalar `SSL Kill Switch 3`
+ Entrar a configuración => "SSL Kill Switch 3" => Disable Certificate Validation

## Pruebas dinámicas

### Frida

Frida requiere dos aplicaciones: 
1. Frida server (instalado en el celular) 
1. Frida client (Instalado en la computadora)
+ Nota: Ambos deben tener la misma versión instalada. Por ejemplo: 16.7.13

#### Instalar cliente de Frida en Debian 12
~~~bash
# Instalar pipx
sudo apt install pipx

# Instalar frida-tools
pipx install frida-tools

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

#### Instalar Frida Server en iOS
1. Frida
	+ Abrir Sileo
	+ Ir a sources => Añadir
		+ `https://build.frida.re` 
	+ Ir a `Search` y buscar `Frida`
	+ Instalar `Frida`
		+ En caso de que no instale presionar los botones: Get => Queque => Confirm => Done

#### Corroborando que todo funcione de manera adecuada

1. Conectas el iPhone a la computadora
1. Desde tu computadora abres la terminal
~~~bash
# Show devices
frida-ls-devices

# Connect Frida to an iPad over USB and list running processes
frida-ps -U

# List running applications
frida-ps -Ua

# List installed applications
frida-ps -Uai

# Connect Frida to the specific device
frida-ps -D 0216027d1d6d3a03
~~~
** En caso del error:
+ `Failed to enumerate processes: no viable transport found`
	+ Corrobora que el dispositivo confie en la computadora
	+ Corrobora que ambos dispositivos corran la misma versión de Frida
	+ Corrobora que el servicio `usbmuxd` se encuentre activo

~~~bash
# Corroborar que el servicio usbmuxd este activo
sudo systemctl status usbmuxd

# En caso de que no este activo, iniciarlo
sudo systemctl start usbmuxd
~~~


### Objection

#### Instalar Objection
+ Es requerido contar con Frida para hacer uso de este módulo

~~~bash
# Instalar pipx
sudo apt install pipx

# Instalar Objection
pipx install objection

# Actualizar objection
pipx upgrade objection

# Quitar objection
pipx uninstall objection
~~~

#### Start Objection and Attach to a Process
1. Conectas el iPhone a la computadora
1. Desde tu computadora abres la terminal
~~~bash
# Show devices
frida-ls-devices

# Indentify installed apps
frida-ps -Uai

# Start Objection and Attach to a Process
objection -g com.app.name explore

##################################################3
# Obtener información

# obtain some binary infos about the app
ios info binary

# dump the iOS key chain
ios keychain dump

# list the app's bundled files
ls

# inspect an app's bundle file
file cat <filename.extension>

# dump iOS shared credentials cache
ios nsurlcredentialstorage dump

# inspect NSUserDefaults class
ios nsuserdefaults get

# dump stored cookies
ios cookies get
~~~

************************************************************************************************
# ByPass Jailbreak Detection
Por probar:
+ https://github.com/jjolano/shadow
+ https://codeshare.frida.re/
+ https://codeshare.frida.re/@incogbyte/ios-jailbreak-bypass/
+ https://codeshare.frida.re/@DevTraleski/ios-jailbreak-detection-bypass-palera1n/
+ https://www.corellium.com/blog/ios-jailbreak-detection-bypass
+ https://github.com/Incognito-Lab/Frida-iOS-Jailbreak-detection-bypass/blob/main/ios-jailbreak-detection-bypass.js
+ https://medium.com/haktrak-cybersecurity-squad/bypassing-ios-jailbreak-detection-by-patching-the-binary-with-ghidra-write-up-of-no-escape-lab-02b417b3a235
+ https://gist.github.com/viizard/12b811da6f128ea544545511320fb277
+ https://www.appknox.com/blog/ios-jailbreak-detection-bypass
+ https://medium.com/@Bug_Fucker/bypassing-jailbreak-detection-using-dopamine-hidejailbreak-f53f02d11994
+ https://arz101.medium.com/ios-pentesting-bypassing-jailbreak-detection-3502de588901


************************************************************************************************
# Comandos útiles
## Encontrar aplicaciones dentro de iOS a través de una conexión ssh
~~~bash
find . 2>/dev/null | grep "app_name"
~~~

## Extracción de IPA

~~~bash
# Te conectas al Iphone
ssh root@<iPhone_IP>

# Entras al directorio de aplicaciones
cd /var/containers/Bundle/Application/

# Encuentras el UUID Folder de tu aplicación
find | grep "<APP_NAME>" | cut -d "/" -f2 | sort -u 

# Entras UUID-Folder
cd <UUID_Folder>

# Creas una carpeta para almacenar tu app
mkdir Payload

# Copia la app a la carpeta temporal
cp -r <APP_NAME>.app/ Payload/

# Crear tu archivo .ipa
zip -r /var/root/<NAME_TO_EXTRACT_IPA>.ipa Payload/

# Descargas la aplicación
sftp root@<iPhone-IP>
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

## Extracción de IPA automatizada
+ Este script se debe correr con `bash`, no con `zsh`
~~~bash
# Te conectas al Iphone
ssh root@<iPhone_IP>

# Instala zip, solamente es necesario correrlo la primera vez
apt install zip

# Indentifica el nombre de tu app. Este debe ser único
find /var/containers/Bundle/Application/ -iname "*.app" | cut -d "/" -f7 | sort 

# Script Automatizado, reemplaza el valor de APPNAME
APPNAME="NAME"; cd /var/containers/Bundle/Application/; if [ $(find | grep $APPNAME | cut -d "/" -f2 | sort -u | wc -l) == "1" ]; then echo "Comenzando respaldo" && UUID_Folder=$(find | grep $APPNAME | cut -d "/" -f2 | sort -u); cd $UUID_Folder; rm -rf Payload; mkdir Payload; ls | grep .app; NAME_TO_EXTRACT_IPA=$(ls | grep .app); cp -r $NAME_TO_EXTRACT_IPA Payload/; IPA_FINAL=$(printf '%s' $(cut -d. -f1 <<< $NAME_TO_EXTRACT_IPA)).ipa; zip -r /var/root/$IPA_FINAL Payload && echo "Guardado como:"; ls /var/root/ | grep "ipa"; echo -e "Para descargar:\n"; echo "scp root@IP:/var/root/$IPA_FINAL ."; sleep 5; exit; else echo "Too many apps with that name"; fi;

# Para descargar el IPA se te mostrará el comando que debes introducir. Similar a
scp root@IP:/var/root/$IPA_FINAL
~~~

************************************************************************************************
# Análisis de código estático y otros
Hopper debbuger
+ https://www.hopperapp.com/download.html

MobSF
+ https://github.com/MobSF/Mobile-Security-Framework-MobSF

Class Dump
+ http://stevenygard.com/projects/class-dump/

xCode
+ https://developer.apple.com/xcode/

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

************************************************************************************************
# Defenses againts hacking

These measures include: 
+ encrypting sensitive data stored within the app
+ testing for local authentication
+ evaluating API vulnerabilities (such as URL schemes, WebViews, etc)
+ safeguarding against manipulation, one of which is the implementation of jailbreak detection mechanisms.

## Tipos de detección de jailbreak
1. File-based Detection
	+ /Applications/Cydia.app
	+ /bin/bash, /usr/sbin/sshd
	+ Dynamic libraries like Substrate
1. Process-based Detection
	+ frida-server
1. Directory Permissions Detection
1. URI Schemes Detection
	+ cydia://


************************************************************************************************
# Anexo - Más información

## Teoría
Penetration testing in rootless
+ https://blog.lrvt.de/rootful-or-rootless-jailbreaks-for-pentesting/
	+ jo-ya

## Para hacer jailbreak

### Información en general
+ https://theapplewiki.com/wiki/Jailbreak

### Palera1n
+ https://github.com/palera1n/palera1n
+ https://palera.in/

### Palera1n Iphone 8
+ https://www.youtube.com/watch?v=cFLvbdyV3WI

### Checkra1n
+ https://checkra.in/

### Dopamine 
+ https://github.com/opa334/Dopamine

### RootHide Dopamine


### TrollStore
+ https://ios.cfw.guide/installing-trollstore/

## Acceder a modo DFU
DFU Iphone 8
+ https://www.wikihow.com/Exit-Dfu-Mode-on-iPhone-8

## Herramientas útiles para realizar pruebas de seguridad
## Instalar Filza
+ https://www.youtube.com/watch?v=5m25eM8tAW0
+ https://www.ios-repo-updates.com/share/tigisoftware/

## CA BurpSuite
+ https://portswigger.net/burp/documentation/desktop/mobile/config-ios-device?ref=blog.lrvt.de

## SSL Kill Switch 2
+ https://github.com/nabla-c0d3/ssl-kill-switch2 

## SSL Kill Switch 3
+ https://github.com/NyaMisty/ssl-kill-switch3
+ https://github.com/NyaMisty/ssl-kill-switch3/releases/download/v1.4/moe.misty.sslkillswitch3_1.4+rootful_iphoneos-arm.deb

## ElleKit
+ https://ellekit.space/

## AppSync Unified
+ https://github.com/akemin-dayo/AppSync

## Frida
+ https://frida.re/docs/ios/
+ https://github.com/frida/frida

## Objection
+ https://www.virtuesecurity.com/kb/ios-frida-objection-pentesting-cheat-sheet/
	+ jo-ya

## Extraer IPA
+ https://awesome.hackpuntes.com/ios/how-to-extract-ipa-from-ios-device


************************************************************************************************
# Tutoriales
Ingeniería reversa para evadir jailbreak detection
+ https://medium.com/haktrak-cybersecurity-squad/bypassing-ios-jailbreak-detection-by-patching-the-binary-with-ghidra-write-up-of-no-escape-lab-02b417b3a235
	+ jo-ya
	+ Herramientas sugeridas
		+ OpenSSH
		+ Zip
		+ Ghidra
			+ Since the term ‘jailbreak’ is sometimes written as ‘jailbroken,’ I recommend searching with just ‘jailbr’ to cover both variations.
		+ Sideloadly
		+ Filza
		+ IPAInstaller
		+ https://github.com/blacktop/ipsw
			+ What if we’re uncertain about which binary to import
+ https://infosecwriteups.com/bypassing-ios-jailbreak-detection-by-patching-the-binary-with-ghidra-write-up-of-no-escape-lab-02b417b3a235

# Casos de uso
+ https://www.sisainfosec.com/blogs/exploiting-idor-in-an-encrypted-mobile-api-with-frida/
+ https://www.guardsquare.com/blog/two-attack-scenarios-will-defeat-your-diy-ios-app-protection

# Cosas que podrían ser de utilidad
+ https://github.com/jjolano/shadow
+ https://highaltitudehacks.com/2018/07/29/ios-application-security-part-53-objection-continued.html

## Labs
+ https://www.mobilehackinglab.com/course/lab-no-escape
+ https://www.mobilehackinglab.com/free-mobile-hacking-labs

## Frida
+ https://www.vaadata.com/blog/frida-the-tool-dedicated-to-mobile-application-security/

## Objection
+ https://medium.com/@SecureWithMohit/exploring-ios-applications-with-frida-and-objection-basic-commands-for-pentesting-4c637dbeb9fd

## Info general
+ https://ios.cfw.guide/blocking-jailbreak-detection/
+ https://theapplewiki.com/wiki/Jailbreak
+ https://ios.cfw.guide/installing-trollstore/

