---
title: Configurando un laboratorio para pruebas de penetración
published: 2024-12-09
description: 'Comandos básicos para Docker'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/linux_laboratorio/portada.png'
tags: [Linux, Hacking]
category: 'Linux'
draft: false 
lang: 'es'
---

# Configurando un laboratorio para pruebas de penetración

## Sistema operativo
+ Debian 12 LTS 
	+ O el más reciente con un kernel estable. Kernels recientes tienen problemas para virtualizar en vmWare, Android Studio y GenyMotion
	+ Linux 6.1.0-27
	+ Instalar build-essential
	+ Linux headers

~~~bash
sudo apt -y install build-essential git vim
sudo apt -y install linux-headers-$(uname -r)
~~~

## Rutas para nuevos programas en PATH
+ /usr/local/bin/
+ /opt/new_program/

### Amenidades para el sistema operativo
+ touchegg - Linux multi-touch gesture recognizer
+ Custom Shortcut for open terminal - gnome-terminal
+ System Monitor Next
	+ `sudo apt install gir1.2-gtop-2.0 gir1.2-nm-1.0 gir1.2-clutter-1.0 gnome-system-monitor`
	+ gnome-shell-extension-manager 
+ Flameshot - flameshot gui
+ Git
+ Linux-headers
+ Build Essentials
+ vim
+ KeePass

## Manejo de evidencia
+ CherryTree en su versión más reciente
+ Flameshot

## Editor de texto
+ vsCode

## Navegadores
+ Brave
+ Google Chrome
+ Firefox

## Administración remota
+ AnyDesk

## Proxy y temas de red
+ BurpSuite
+ Wireshark

## Virtualizadores
+ VMware® Workstation 17 Pro 17.6.0 (O la más reciente y estable)
+ Docker
+ GenyMotion
+ Android Studio
	+ https://wiki.debian.org/AndroidStudio
	+ https://medium.com/@dmaioni/android-studio-62ab2f54911c

## Visor de bases de datos

~~~bash
sudo apt-get install sqlitebrowser
~~~
+ https://sqlitebrowser.org/dl/

## Máquinas virtuales
+ Windows 10
+ Kali Linux
+ Parrot OS

## Imágenes de Docker
+ MobSF
+ NextCloud

## Password Cracking
+ Hashcat
	+ CUDA drivers for graphics cards

## ScreenRecorder
+ Simple Screen Recorder

## Web
+ whatweb
+ dirb

## Misc
+ exiftool

# Anexo - Fuentes y más información
Amenidades para el sistema operativo
+ https://github.com/JoseExposito/touchegg?tab=readme-ov-file
+ https://github.com/priyaranjankumar/touchegg.conf

Manejo de evidencia
+ https://flameshot.org/

Editor de texto
+ https://code.visualstudio.com/

Virtualizadores
+ https://blogs.vmware.com/workstation/2024/05/vmware-workstation-pro-now-available-free-for-personal-use.html
+ https://www.docker.com/
+ https://www.genymotion.com/ 
+ https://developer.android.com/studio/?hl=es-419 

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