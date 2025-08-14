---
title: Instalando un adaptador USB WIFI en VirtualBox
published: 2020-04-26
description: '¿Cómo instalar un adaptador USB WIFI en VirtualBox?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/wifi_virtualbox/1.webp'
tags: [Linux]
category: 'Linux'
draft: false 
lang: 'es'
---

Al realizar procesos de auditoría WIFI puede ser necesario utilizar máquinas virtuales, para esto, es posible usar una máquina en VirtualBox y un adaptador de WIFI vía USB.

Sin embargo, es común que cuando se realiza esto sobre una máquina Linux, el adaptador WIFI-USB no sea reconocido por la máquina virtual.

Para solucionar esto podemos realizarlo siguiente:

:::note[Entorno de prueba]
+ Host (Máquina física): Ubuntu 20.04 LTS
+ Guest (VirutualBox): Kali Linux 2020
+ Antena: TPLINK (TL-WN823N)
:::

## Instalación de dependencias:

1. Instalación de extensiones de VirtualBox
1. Añadir usuario al grupo vboxusers

~~~bash
sudo apt -y install virtualbox-ext-pack
sudo usermod -aG vboxusers _username_
~~~

## Reinicio del sistema y configuración de VirtualBox

1. Reiniciamos el sistema
1. Abrimos nuestra máquina en Virtual Box
1. Conectamos nuestro USB WIFI

En la parte inferior derecha podremos encontrar un icono de USB, al darle click derecho podremos seleccionar nuestra USB. En este caso 802.11 USB WLAN

<img width=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/wifi_virtualbox/1.webp">

Lo activamos

<img width=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/wifi_virtualbox/2.webp">

En nuestra máquina virtual podremos ver que en el icono de red ya es reconocida nuestro adaptador USB WIFI

<img width=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/wifi_virtualbox/3.webp">

Finalmente, estamos conectados a la red deseada.

<img width=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/wifi_virtualbox/4.webp">

Con esto ya tenemos configurado nuestra máquina virtual en VirtualBox con un adaptador de WIFI via USB y podremos realizar cualquier proceso que deseemos, por ejemplo, utilizar la suite de *aircrack-ng*.

:::tip[Nota final]
Gracias por terminar de leer este artículo.

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>