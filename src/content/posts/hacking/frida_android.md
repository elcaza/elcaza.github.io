---
title: Frida en Android
published: 2026-01-14
description: '¿Cómo instalar y usar Frida en Android?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/frida_android/portada.jpg'
tags: [Hacking, Mobile, Android]
category: 'Hacking'
draft: false 
lang: 'es'
---

# 1. ¿Qué es Frida?
Frida es un toolkit de instrumentación dinámica. Permite "inyectar" código (generalmente scripts en JavaScript) dentro de una aplicación que ya se está ejecutando en un dispositivo Android o iOS, para observar o modificar su comportamiento en tiempo real sin necesidad de tener el código fuente ni de recompilar la app.

# 2. Detalles importantes a considerar (Errores con algunas versiones)
Algunas versiones de Frida son incompatibles con Objection, por lo que debes instalar una versión específica de Frida.
Los siguientes enlaces dan más detalles al respecto, pero este post abarcará la instalación que hasta enero del 2026 me ha funcionado.
+ <a href="https://github.com/frida/frida/issues/2897" target="_blank">Github</a>
+ <a href="https://infosecwriteups.com/the-frida-objection-setup-guide-solving-version-hell-on-android-ios-timeless-guide-f55eb98459a0" target="_blank">The Frida & Objection Setup Guide: Solving Version Hell on Android & iOS (Timeless Guide)</a>
+ <a href="https://freedium.cfd/https://infosecwriteups.com/the-frida-objection-setup-guide-solving-version-hell-on-android-ios-timeless-guide-f55eb98459a0" target="_blank">The Frida & Objection Setup Guide: Solving Version Hell on Android & iOS (Timeless Guide) - Freedium</a>

# 3. Instalación de Frida
Para usar Frida requieres lo siguiente:

+ Frida Server
    + Instalada en el celular (Android con root)
+ Frida Client
    + Instalada en la computadora
+ Ambas versiones deben coincidir y estar ejecutándose en tus dispositivos

## 3.1 Instalación de UV

~~~bash
curl -LsSf https://astral.sh/uv/install.sh | sh

    # source $HOME/.local/bin/env (sh, bash, zsh)
    # source $HOME/.local/bin/env.fish (fish)
~~~

## 3.2 Instalación de Frida client (computadora)

Instalar Frida en una versión en específico
+ Compatible con Objection y Frida 16.5.2

~~~bash
mkdir 16.5.2 && cd 16.5.2

# Inicializa el proyecto uv (sin añadir tu nombre y correo de Git)
uv init --author-from none

# Añade frida, frida-tools y objection
uv add frida==16.5.2 frida-tools objection==1.12.3

# Activa el entorno virtual
source .venv/bin/activate

# Verifica la versión instalada
frida --version
~~~
- Para una versión de python en específico
  - `uv init --author-from none --python 3.12`
- Si no vas a usar Objection, puedes instalar una versión reciente de Frida
  - `uv add frida frida-tools`

Opcionalmente puedes crear un alias

~~~bash
echo "alias uvs='source .venv/bin/activate'" >> ~/.bashrc
~~~

Recuerda que estás en un entorno virtual gestionado por `uv`, para salir del entorno virtual
~~~bash
deactivate
~~~

## 3.3 Instalación de Frida server (celular)

### 1) Conoce tu arquitectura

~~~bash
adb shell getprop ro.product.cpu.abi
~~~
+ ARM32 => `armeabi-v7a` => usa `frida-server-${VERSION}-android-arm.xz`
+ ARM64 => `arm64-v8a` => usa `frida-server-${VERSION}-android-arm64.xz`
+ x86_64 => `x86_64` => usa `frida-server-${VERSION}-android-x86_64.xz`
+ x86 => `x86` => usa `frida-server-${VERSION}-android-x86.xz`

### 2) Descarga Frida Server para Android

~~~bash
# La versión debe coincidir con la que usarás en tu computadora
VERSION=16.5.2

# ARM 32bits
wget https://github.com/frida/frida/releases/download/${VERSION}/frida-server-${VERSION}-android-arm.xz

# ARM 64bits
wget https://github.com/frida/frida/releases/download/${VERSION}/frida-server-${VERSION}-android-arm64.xz
~~~
+ Descarga solo el de tu arquitectura.

### 3) Descomprime y copia al celular

~~~bash
# Descomprimir 32bits
xz -dc frida-server-${VERSION}-android-arm.xz > frida-server-${VERSION} && chmod +x frida-server-${VERSION}

# Descomprimir 64bits
xz -dc frida-server-${VERSION}-android-arm64.xz > frida-server-${VERSION} && chmod +x frida-server-${VERSION}

# Copiar al celular
adb push frida-server-${VERSION} /data/local/tmp/
~~~

### 4) Ejecuta Frida server

~~~bash
# Si no conservó los permisos de ejecución
adb shell su -c "chmod 755 /data/local/tmp/frida-server-${VERSION}"

adb shell su -c "/data/local/tmp/frida-server-${VERSION} &"
~~~

Instalación con Frida Launcher
+ <a href="https://github.com/thecybersandeep/Frida-Launcher" target="_blank">Frida Launcher</a>

### 5) Verifica que el servicio esté corriendo
~~~bash
adb shell su -c "pgrep -l frida-server"
~~~

### 6) Ver la versión
~~~bash
adb shell su -c "/data/local/tmp/frida-server-${VERSION} --version"
~~~

### Detener frida

~~~bash
adb shell su -c "pkill frida-server"
~~~

# 4. Comandos de Frida

## 4.1 Dentro del entorno virtual

### Listado de dispositivos

~~~bash
frida-ls-devices
~~~

#### En caso de error

Si tu celular no aparece, verifica que `adb` lo detecte y acepta la autorización de depuración USB en el celular.

~~~bash
adb devices
~~~
- vuelves a correr `frida-ls-devices`

### Listado de apps instaladas

~~~bash
frida-ps -Uai
~~~

### Ejecución de un script

~~~bash
frida -U -f com.app.app -l script.js
~~~

## 4.2 Ejecuciones rápidas - fuera del entorno virtual

### Frida + script
Puedes usar un entorno efímero con uv. Por ejemplo, para correr un script.
~~~bash
FRIDA_VERSION=16.5.2
OBJECTION_VERSION=1.12.3
APP_NAME="com.app.app"

uv run --with frida==$FRIDA_VERSION --with frida-tools --with objection==$OBJECTION_VERSION frida -U -f $APP_NAME -l script.js
~~~

### Frida ls devices
Puedes usar un entorno efímero con uv. Por ejemplo, para listar dispositivos y apps.
~~~bash
FRIDA_VERSION=16.5.2

uv run --with frida==$FRIDA_VERSION --with frida-tools frida-ls-devices

uv run --with frida==$FRIDA_VERSION --with frida-tools frida-ps -Uai
~~~

# Anexo - Objection y versiones de Frida (probadas)

Objection no funciona de forma consistente en todas las versiones de Frida.
Las combinaciones probadas en iOS están en <a href="https://elcaza.github.io/posts/hacking/frida_ios/" target="_blank">Frida en iOS</a>.

En Android, la combinación que me funcionó fue:
+ Python 3.11.2
+ frida v16.5.2
+ frida-tools v13.7.1
+ objection v1.11.0

+ Si no inicia a la primera <a href="https://elcaza.github.io/posts/hacking/objection/" target="_blank">objection</a> puedes abrir la app y volverlo a correr

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
