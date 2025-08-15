---
title: Apuntes
published: 2025-08-11
description: 'Description'
image: ''
tags: [Apuntes]
category: 'Apuntes'
draft: false 
lang: 'es'
---

# Setting up laboratory for pentesting

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
    + sudo apt install gir1.2-gtop-2.0 gir1.2-nm-1.0 gir1.2-clutter-1.0 gnome-system-monitor
    + gnome-shell-extension-manager 
+ Flameshot - flameshot gui
+ Git
+ Linux-headers
+ Build Essentials
+ vim
+ KeePass

## Manejo de evidencia
+ CherryTree 1.2.0
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
+ VMware® Workstation 17 Pro 17.6.0
+ Docker
+ GenyMotion
+ Android Studio
    + https://wiki.debian.org/AndroidStudio
    + https://medium.com/@dmaioni/android-studio-62ab2f54911c

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

************************************************************************************************
# Utilidades

## Compartir carpeta en máquina virtual (Linux host - Linux guest)
+ https://www.youtube.com/watch?v=PSG8ka2CkaQ
~~~bash
# Para montaje manual de la carpeta compartida:
sudo mount -t fuse.vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other

# Para averiguar si esta compartida la carpeta: 
vmware-hgfsclient
~~~

************************************************************************************************
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

************************************************************************************************
# Notas generales sobre Linux

## Variables
~~~bash
# Variable path
export PATH=$PATH:/new_route
# También puede ser
export PATH=new_route:$PATH
~~~

## Cargar variables
~~~bash
# Dependiendo de la ruta que quieras como origen
source ~/.profile 
source ~/.bashrc
~~~

## Para guardar la variables de forma permanente
+ `/etc/profile` (for all users)
+ `~/.bash_profile` (for current user)
+ `~/.bash_login` (for current user)
+ `~/.profile` (for current user)
+ ` ~/.bashrc` (for current user)
+ `/etc/environment` to set a permanent PATH environment variable, but it does not support variable expansion.

Más información:
+ https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix
+ https://www.sysadmit.com/2016/06/linux-anadir-ruta-al-path.html

## Administración básica
~~~bash
# Crear un grupo
groupadd grupo

# Crear usuarios
    # adduser => Script
    # useradd => Manual

useradd -ms /bin/bash superadmin
# -m Crea carpeta home
# -s Asigna shell

# Añadir un usuario a grupo
usermod -a -G sudo soporte

# Cambia password
passwd superadmin

## Agregamos contraseña al usuario appuser
RUN echo 'user:password' | chpasswd

~~~

### Información del sistema
~~~bash
# Check system version
lsb_release -a
cat /etc/issue
cat /etc/os-release
hostnamectl
~~~

### Teclado
~~~bash
# 1) Configurar teclado
dpkg-reconfigure keyboard-configuration

# 2) Aplicar la configuración del teclado (solo funciona para esa sesión)
setupcon

# 3) Para un cambio permantente
dpkg-reconfigure console-setup
~~~

## Descargar archivos

### wget

~~~bash
# Descarga un archivo
wget <URL>

# Descarga y renombra un archivo
wget -O <filename> <URL>

# Descarga varios archivos desde un archivo de texto con las urls
wget -i download_files.txt

# Descarga una carpeta
wget -r ftp://server-address.com/directory

# Descargar de manera recursiva esos elementos
wget -nd -r -A pdf,doc,docx,xls,xlsx,jpg www.rediris.es
~~~

### curl

~~~bash
# Descarga un archivo
curl --location --request GET 'https://github.com/frida/frida/releases/download/17.2.11/frida-server-17.2.11-android-arm.xz' --output file.name
~~~

## Otros
+ https://docs.google.com/spreadsheets/d/1GTr8VJnxgVRcaV9JFBishw8hIH69f4wVwrlBYsyhAdY/edit?gid=0#gid=0

## Exfiltración de información

### Archivos importantes
+ /etc/passwd
+ /etc/shadow
+ /etc/resolv.conf
+ /etc/hosts
+ /etc/hostname

### Comprimir información 
~~~bash
# Respaldo omitiendo erroes, tipos de archivos y carpetas
tar --warning=no-file-changed --exclude={*.mp4,*.mp3,'./public_html'} --ignore-failed-read -zcvf respaldo.tar.gz .
~~~

************************************************************************************************
# Entornos virtuales en python

## Python Virtual Environment (venv)
~~~bash
# Virtual Environment Installation
python -m venv esptoolenv
# esptoolenv = name

# Activate Virtual Environment
source esptoolenv/bin/activate

# deactivate Virtual Environment
deactivate

# Borrar rm -r esptoolenv en la carpeta

# Instalar frida-tools <herramienta>
pip install frida-tools

# Instalar una versión en especifica
pip install frida-tools==16.7.13
~~~

## Python Virtual Environment (pipx)
~~~bash
# Instalar pipx
sudo apt install pipx

# Instalar frida-tools <herramienta>
pipx install frida-tools

# Instalar una versión en especifica
pipx install frida-tools==16.7.13

# Solamente si no lo detecta el PATH
pipx ensurepath
	# Note: '/home/user/.local/bin' is not on your PATH environment variable. These apps will not be globally accessible until your PATH is updated. Run `pipx ensurepath` to automatically add it, or manually

# Obtener la versión de Frida
 frida --version
	# 16.7.13

# Actualizar frida-tools <herramienta>
pipx upgrade frida-tools

# Desinstalar frida-tools <herramienta>
pipx uninstall frida-tools
~~~

************************************************************************************************
# Utilerías

## Read a do anything
~~~bash
#!/bin/bash
while IFS= read -r line
do
    user=$(echo $line | cut -d ":" -f1)
    password=$(echo $line | cut -d ":" -f2 | base64 -d | base64 -d)
    echo "$user:$password"
done < $1
~~~

## Insertar caracteres antes de una línea
~~~bash
awk '{print "Insertar al inicio " $0 " insertar al final\n"}' archivo.txt
~~~

## Dividir un archivo en varios
~~~bash
split -l 200000 -d --additional-suffix=.txt input_file.txt output_file
~~~

## Imprimir todo lo que no es comentario
~~~bash
awk '!/#/' file
~~~
Referencias:
+ https://www.unixtutorial.org/awk-delimiter/

## Remover caracteres repetidos
~~~bash
tr -s 'character'
tr -s ' '
tr -s '\n'

~~~ 
## Borrar un caracter de un string
~~~bash
tr -d ".|,"
~~~

## Reemplazar una cadena por nada 
~~~bash
sed "s/|a|//g" 
sed "s/|a|/~b~/g" 
~~~

## Remover la última letra del string
~~~bash
sed 's/.$//'
~~~

## Descargar un archivo con scp
~~~bash
scp user@192.168.100.100:path/file.txt /local/dir
~~~

## Subir un archivo con scp
~~~bash
scp file user@192.168.100.100:path/file.txt
~~~

## Descarga con FTP
~~~bash
sftp root@<IP>
get -r /var/root/<file>.txt
~~~

## Con llave publica
~~~bash
scp -i key_file.pem user@192.168.100.100:path/file.txt /local/dir
~~~

## Control con systemctl
~~~bash
# Systemctl
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh

# Preguntar si está activo
sudo systemctl is-active apache

# Preguntar si está habilitado
sudo systemctl is-enable apache
~~~

************************************************************************************************
# Hexedit
+ Utiliza Ctrl-S para buscar hacia adelante y Ctrl-R para buscar hacia atrás. 
+ Usa Inicio, Fin, AvPág y Repág para movimientos más amplios. 
+ Guardar los cambios: Presiona F2 para guardar los cambios realizados en el archivo. 
+ Salir del programa: Normalmente, se sale presionando Ctrl-X o cerrando la ventana de la terminal. 

:::tip[Nota final]
¡Gracias por terminar de leer este artículo! uwur

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>