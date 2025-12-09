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
sudo apt -y install build-essential git vim curl gparted cherrytree flameshot net-tools whatweb dirb exiftool
sudo apt -y install linux-headers-$(uname -r)
~~~

## Rutas para nuevos programas en PATH
+ `/usr/local/bin/`
+ `/opt/new_program/`

## Amenidades para el sistema operativo
### KeePassXC
+ `sudo apt install keepassxc`

### Custom Shortcut for open terminal - gnome-terminal
+ `gnome-terminal`

### Flameshot - flameshot gui
+ `flameshot gui`
+ `flameshot launcher`

### System Monitor Next
~~~bash
sudo apt install gir1.2-gtop-2.0 gir1.2-nm-1.0 gir1.2-clutter-1.0 gnome-system-monitor
~~~
+ Terminas la instalación a través de <a href="https://extensions.gnome.org/extension/3010/system-monitor-next/" target="_blank">System Monitor Next</a>

### touchegg - Linux multi-touch gesture recognizer
+ <a href="https://github.com/JoseExposito/touchegg/releases" target="_blank">touchegg_2.0.18_amd64.deb</a>

~~~bash
sudo dpkg -i touchegg_2.0.18_amd64.deb
sudo systemctl start touchegg
sudo systemctl enable touchegg

wget https://raw.githubusercontent.com/priyaranjankumar/touchegg.conf/refs/heads/main/touchegg.conf

mkdir ~/.config/touchegg/
cp touchegg.conf ~/.config/touchegg/
# Reinica el sistema y debe funcionar
~~~

## Manejo de evidencia
+ CherryTree en su versión más reciente
+ Flameshot

## Editor de texto
+ <a href="https://code.visualstudio.com/" target="_blank">vsCode</a>
	+ Plugin para el keymap de SublimeText

## Navegadores
+ Brave
+ Google Chrome
+ Firefox

## Administración remota
+ AnyDesk

## Multimedia
+ VLC
	+ `sudo apt install vlc`

## Proxy y temas de red
+ BurpSuite
+ Wireshark
	+ `sudo apt install wireshark`

## Virtualizadores

### VMware® Workstation 17 Pro 17.6.0 (O la más reciente y estable)
+ <a href="https://blogs.vmware.com/workstation/2024/05/vmware-workstation-pro-now-available-free-for-personal-use.html" target="_blank">vmware</a>

### GenyMotion
+ <a href="https://www.genymotion.com/" target="_blank">Genymotion</a>

### Android Studio
1. <a href="https://developer.android.com/studio#downloads" target="_blank">Descarga tar.gz</a>
2. Ejecuta lo siguiente

~~~bash
tar -xzvf android-studio-*.tar.gz
sudo mv android-studio /opt
/opt/android-studio/bin/studio.sh
~~~

3. Crea un acceso directo

`~/.local/share/applications/android_studio.desktop`

~~~bash
[Desktop Entry]
Name=Android Studio
Comment=Android Studio
Exec=/opt/android-studio/bin/studio.sh
Icon=/opt/android-studio/bin/studio.png
Terminal=false
Type=Application
Categories=Development;IDE;
~~~

+ <a href="https://wiki.debian.org/AndroidStudio" target="_blank">Android Studio - Wiki Debian</a>
+ <a href="https://medium.com/@dmaioni/android-studio-62ab2f54911c" target="_blank">Android Studio - Medium</a>

### Docker
+ <a href="https://docs.docker.com/engine/install/debian/" target="_blank">How to install - Docker</a>

~~~bash
# Remover versiones pasadas
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-doc podman-docker containerd runc | cut -f1)

# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Probar
sudo docker run hello-world
~~~

## Visor de bases de datos
~~~bash
sudo apt install sqlitebrowser
~~~
+ <a href="https://sqlitebrowser.org/dl/" target="_blank">Sitio web</a>

## Máquinas virtuales
+ Windows 10
+ Kali Linux
+ Parrot OS

## Imágenes de Docker
+ MobSF
	+ `sudo docker run -it --name mobsf -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest`
	+ `sudo docker start mobsf`
+ NextCloud

## Password Cracking
+ Hashcat
	+ CUDA drivers for graphics cards

## ScreenRecorder
+ Simple Screen Recorder
	+ `sudo apt install simplescreenrecorder`

## Web
+ whatweb
+ dirb

## Misc
+ exiftool

# Anexo - Links
## Amenidades para el sistema operativo
+ <a href="https://github.com/JoseExposito/touchegg?tab=readme-ov-file" target="_blank">JoseExposito - touchegg</a>
+ <a href="https://github.com/priyaranjankumar/touchegg.conf" target="_blank">priyaranjankumar - touchegg.conf</a>

## Manejo de evidencia
+ <a href="https://flameshot.org/" target="_blank">Flameshot</a>

## Virtualizadores
+ <a href="https://blogs.vmware.com/workstation/2024/05/vmware-workstation-pro-now-available-free-for-personal-use.html" target="_blank">vmware</a>
+ <a href="https://www.docker.com/" target="_blank">Docker</a>
+ <a href="https://www.genymotion.com/" target="_blank">Genymotion</a>
+ <a href="ttps://developer.android.com/studio/?hl=es-419" target="_blank">Android Studio</a>


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