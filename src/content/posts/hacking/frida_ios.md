---
title: Frida en iOS
published: 2026-01-14
description: '¿Cómo instalar y usar Frida en iOS?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/frida/portada.jpg'
tags: [Hacking, iOS]
category: 'Hacking'
draft: false 
lang: 'es'
---

# 1. ¿Qué es Frida?
Frida es un toolkit de instrumentación dinámica. Permite "inyectar" código (generalmente scripts en JavaScript) dentro de una aplicación que ya se está ejecutando en un dispositivo Android o iOS, para observar o modificar su comportamiento en tiempo real sin necesidad de tener el código fuente ni de recompilar la app.

# 2. Detalles importantes a considerar (Errores con algunas versiones)
Algunas versiones de Frida son incomptatibles con Objection, por lo que debes instalar una versión especifica de Frida.
Los siguientes enlaces dan más detalles al respecto, pero este post abarcará la instalación que hasta enero del 2026 me ha funcionado.
+ <a href="https://github.com/frida/frida/issues/2897" target="_blank">Github</a>
+ <a href="https://infosecwriteups.com/the-frida-objection-setup-guide-solving-version-hell-on-android-ios-timeless-guide-f55eb98459a0" target="_blank">The Frida & Objection Setup Guide: Solving Version Hell on Android & iOS (Timeless Guide)</a>
+ <a href="https://freedium.cfd/https://infosecwriteups.com/the-frida-objection-setup-guide-solving-version-hell-on-android-ios-timeless-guide-f55eb98459a0" target="_blank">The Frida & Objection Setup Guide: Solving Version Hell on Android & iOS (Timeless Guide) - Freedium</a>

# 3. Instalación de Frida
Para usar Frida requieres lo siguiente:

+ Frida Server
    + Instalada en el celular (iOS)
+ Frida Client
    + Instalada en la computadora
+ Ambas versiones deben coincidir y estar ejecutandose en tus dispositivos

## 3.1 Instalación de UV

~~~bash
curl -LsSf https://astral.sh/uv/install.sh | sh

    # source $HOME/.local/bin/env (sh, bash, zsh)
    # source $HOME/.local/bin/env.fish (fish)
~~~

## 3.2 Instalación de Frida client (computadora)

Instalar Frida en una versión en especifico 
+ Compatible con Objection y Frida 16.5.2

~~~bash
mkdir 16.5.2 && cd 16.5.2

# Inicializa el proyecto uv
uv init

# Añade frida, frida-tools y objection
uv add frida==16.5.2 frida-tools objection==1.12.3

# Activa el entorno virtual
source .venv/bin/activate

# Verifica la versión instalada
frida --version
~~~
- Para una versión de python en especifico
  - `uv init --python 3.12`


Opcionalmente puedes crear un alias

~~~bash
echo "alias uvs='source .venv/bin/activate'" >> ~/.bashrc
~~~

Recuerda que estás en un entorno virtual gestionado por `uv`, para salir del entorno virtual
~~~bash
deactivate
~~~

## 3.3 Instalación de Frida server (celular)

### 1) Obten la versión
~~~bash
IP=10.1.1.1
ssh root@$IP
~~~

### 2) Desinstalas cualquier instalación previa de Frida
~~~bash
sudo dpkg -r frida; sudo dpkg -P re.frida.server
~~~

### 3) Descarga Frida Server para iOS

~~~bash
# La versión debe coincidir con la que usarás en tu computadora
VERSION=16.5.2

# ARM rootful
wget https://github.com/frida/frida/releases/download/${VERSION}/frida_${VERSION}_iphoneos-arm.deb

# ARM 64 rootless
wget https://github.com/frida/frida/releases/download/${VERSION}/frida_${VERSION}_iphoneos-arm64.deb
~~~
- Si no tienes `wget` instalado puedes instalarlo con `sudo apt install wget`

### 4) Instalas la versión especifica
~~~bash
# Rootful
sudo dpkg -i frida_${VERSION}_iphoneos-arm.deb

# Rootless
sudo dpkg -i frida_${VERSION}_iphoneos-arm64.deb
~~~
+ Verifica que sea tu archivo. Variará acorde a la versión.

### 5) Verifica que el servicio esté corriendo
~~~bash
dpkg -L re.frida.server | grep frida-server
~~~

### 6) Ver la versión
~~~bash
frida-server --version
~~~

# 4. Comandos de Frida

## 4.1 Dentro del entorno virtual 

### Listado de dispositivos 

~~~bash
frida-ls-devices
~~~

#### En caso de error

El error:

~~~txt
6db3dae8840d746d090d5bcffa20f92ba2fffeee  usb     iOS Device

Segmentation fault
~~~

Solución:

~~~bash
sudo systemctl restart usbmuxd.service 
~~~
- vuelves a correr `frida-ls-devices`

### Listado de apps instaladas 

~~~bash
frida-ps -Uai
~~~

### Ejecución de un script

~~~bash

~~~

## 4.2 Ejecuciones rápidas - fuera del entorno virtual

### Frida + script
Puedes correr una especie de contenedor efimero con uv. Por ejemplo, para correr un script.
~~~bash
FRIDA_VERSION=16.5.2
OBJECTION_VERSION=1.11.0
APP_NAME="com.app.app"

uv run --with frida==$FRIDA_VERSION --with frida-tools --with  objection==$OBJECTION_VERSION frida -U -f $APP_NAME -l script.js
~~~

### Frida ls devices
Puedes correr una especie de contenedor efimero con uv. Por ejemplo, para correr un script.
~~~bash
FRIDA_VERSION=16.5.2

uv run --with frida==$FRIDA_VERSION --with frida-tools frida-ls-devices

uv run --with frida==$FRIDA_VERSION --with frida-tools frida-ps -Uai
~~~

# Anexo - Objection y versiones de Frida (probadas)

Objection no funciona de forma consistente en todas las versiones de Frida y versiones de iOS.
Según mis pruebas, este es el "resumen confiable"

<table>
  <thead>
    <tr>
      <th>Dispositivo</th>
      <th>Versión de iOS</th>
      <th>Jailbreak</th>
      <th>Passcode</th>
      <th>Versión Frida</th>
      <th>Versión Objection</th>
      <th>ios keychain dump</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>iPad 7th</td>
      <td>18.7.4</td>
      <td>rootful palera1n</td>
      <td>No, nunca</td>
      <td>17.8.0</td>
      <td>objection: 1.12.3</td>
      <td>Solo funcionó una vez</td>
    </tr>
    <tr>
      <td>iPhone 8 - Negro</td>
      <td>16.7.14</td>
      <td>rootful palera1n</td>
      <td>No, nunca</td>
      <td>16.5.2</td>
      <td>objection: 1.12.3</td>
      <td>Sí</td>
    </tr>
    <tr>
      <td>iPhone 8 - Blanco</td>
      <td>16.7.11</td>
      <td>rootful palera1n</td>
      <td>No, nunca</td>
      <td>16.5.2</td>
      <td>objection: 1.12.3</td>
      <td>Sí</td>
    </tr>
  </tbody>
</table>

+ Si no inicia a la primera `objection` puedes abrir la app y volverlo a correr

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