---
title: Frida en iOS y Android
published: 2026-01-13
description: '¿Cómo usar Frida en iOS y Android?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/frida/portada.jpg'
tags: [Hacking, Android, iOS]
category: 'Hacking'
draft: true 
lang: 'es'
---

# ¿Qué es Frida?
Frida es un toolkit de instrumentación dinámica. Permite "inyectar" código (generalmente scripts en JavaScript) dentro de una aplicación que ya se está ejecutando en un dispositivo Android o iOS, para observar o modificar su comportamiento en tiempo real sin necesidad de tener el código fuente ni de recompilar la app.

# Detalles importantes a considerar (Errores con algunas versiones)
Algunas versiones de Frida son incomptatibles con Objection, por lo que debes instalar una versión especifica de Frida.
Los siguientes enlaces dan más detalles al respecto, pero este post abarcará la instalación que hasta enero del 2026 me ha funcionado.
+ <a href="https://github.com/frida/frida/issues/2897" target="_blank">Github</a>
+ <a href="https://infosecwriteups.com/the-frida-objection-setup-guide-solving-version-hell-on-android-ios-timeless-guide-f55eb98459a0" target="_blank">The Frida & Objection Setup Guide: Solving Version Hell on Android & iOS (Timeless Guide)</a>
+ <a href="https://freedium.cfd/https://infosecwriteups.com/the-frida-objection-setup-guide-solving-version-hell-on-android-ios-timeless-guide-f55eb98459a0" target="_blank">The Frida & Objection Setup Guide: Solving Version Hell on Android & iOS (Timeless Guide) - Freedium</a>

# Instalación de Frida
Para usar Frida requieres lo siguiente:

+ Frida Server
    + Instalada en el celular (Android o iOS)
+ Frida Client
    + Instalada en la computadora
+ Ambas versiones deben coincidir y estar ejecutandose en tus dispositivos

## Instalación de Frida Client con UV

### Instalar uv
~~~bash
curl -LsSf https://astral.sh/uv/install.sh | sh

    # source $HOME/.local/bin/env (sh, bash, zsh)
    # source $HOME/.local/bin/env.fish (fish)
~~~

### Instalar Frida
~~~bash
mkdir frida_uv && cd frida_uv

uv venv

source .venv/bin/activate

VERSION=16.5.2
uv pip install frida==$VERSION frida-tools objection

~~~
+ Puedes probar diferentes versiones.
+ La versión 16.5.2 es compatible con objection

### Para salir
~~~bash
deactivate
~~~

### Protip
Puedes correr una especie de contenedor efimero con uv. Por ejemplo, para correr un script.
~~~bash
VERSION=16.5.2
uv run --with frida==$VERSION --with frida-tools --with objection frida -U -f com.app.app -l script.js
~~~

## iOS - Instalar una versión especifica de Frida

### 1) Obten la versión
~~~bash
frida --version
~~~

### 2) Descarga Frida Server para iOS

~~~bash
VERSION=16.5.2

# ARM 32bits
curl -LO https://github.com/frida/frida/releases/download/${VERSION}/frida_${VERSION}_iphoneos-arm.deb

# ARM 64bits
curl -LO https://github.com/frida/frida/releases/download/${VERSION}/frida_${VERSION}_iphoneos-arm64.deb

# Transferir al dispositivo 32bits
scp frida_${VERSION}_iphoneos-arm.deb root@DEVICE_IP:/tmp/

# Transferir al dispositivo 64bits
scp frida_${VERSION}_iphoneos-arm64.deb root@DEVICE_IP:/tmp/
~~~

### 3) Ingresas al dispositivo vía SSH
~~~bash
ssh root@DEVICE_IP
~~~

### 4) Desinstalas cualquier instalación previa de Frida
~~~bash
dpkg -r frida
dpkg -P re.frida.server
~~~

### 5) Instalas la versión especifica
~~~bash
dpkg -i /tmp/frida_16.5.2_iphoneos-arm64.deb
~~~
+ Verifica que sea tu archivo. Variará acorde a la versión.

### 6) Verifica que el servicio esté corriendo
~~~bash
dpkg -L re.frida.server | grep frida-server
~~~

## Android - Instalar una versión especifica de Frida

### 1) Conoce tu arquitectura
~~~bash
adb shell getprop ro.product.cpu.abi
~~~
+ `arm64-v8a` => use `frida-server-16.5.2-android-arm64.xz`
+ `armeabi-v7a` => use `frida-server-16.5.2-android-arm.xz`
+ `x86_64` => use `frida-server-16.5.2-android-x86_64.xz`
+ `x86` => use `frida-server-16.5.2-android-x86.xz`

### 2) Descarga Frida Server para Android

#### Forma manual
~~~bash
VERSION=16.5.2

# ARM 32bits
wget https://github.com/frida/frida/releases/download/${VERSION}/frida-server-${VERSION}-android-arm.xz

# ARM 64bits
wget https://github.com/frida/frida/releases/download/${VERSION}/frida-server-${VERSION}-android-arm64.xz

# Descomprimir 32bits
xz -dc frida-server-${VERSION}-android-arm.xz > frida-server && chmod +x frida-server

# Descomprimir 64bits
xz -dc frida-server-${VERSION}-android-arm64.xz > frida-server && chmod +x frida-server

## Push Device
adb push frida-server /data/local/tmp/

## Permisos y ejecución
# adb shell chmod 777 /data/local/tmp/frida-server
adb shell su -c "/data/local/tmp/frida-server &"
~~~

#### Instalación con Frida Launcher
+ <a href="github.com/thecybersandeep/Frida-Launcher" target="_blank">Frida Launcher</a>

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