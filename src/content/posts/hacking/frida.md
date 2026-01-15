---
title: Frida en iOS y Android
published: 2026-01-14
description: '¿Cómo usar Frida en iOS y Android?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/frida/portada.jpg'
tags: [Hacking, Android, iOS]
category: 'Hacking'
draft: false 
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
mkdir frida_16.5.2_uv && cd frida_16.5.2_uv

uv venv

source .venv/bin/activate

FRIDA_VERSION=16.5.2
OBJECTION_VERSION=1.11.0
uv pip install frida==$FRIDA_VERSION frida-tools objection==OBJECTION_VERSION

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
FRIDA_VERSION=16.5.2
OBJECTION_VERSION=1.11.0

uv run --with frida==$FRIDA_VERSION --with frida-tools --with  objection==OBJECTION_VERSION frida -U -f com.app.app -l script.js
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

# Logs de versiones

Por si un día falla, como funcionaba era:
+ Python 3.11.2
+ frida v16.5.2
+ frida-tools v13.7.1
+ objection v1.11.0

~~~bash
# uv pip tree
objection v1.11.0
├── click v8.3.1
├── delegator-py v0.1.1
│   └── pexpect v4.9.0
│       └── ptyprocess v0.7.0
├── flask v3.1.2
│   ├── blinker v1.9.0
│   ├── click v8.3.1
│   ├── itsdangerous v2.2.0
│   ├── jinja2 v3.1.6
│   │   └── markupsafe v3.0.3
│   ├── markupsafe v3.0.3
│   └── werkzeug v3.1.5
│       └── markupsafe v3.0.3
├── frida v16.5.2
├── frida-tools v13.7.1
│   ├── colorama v0.4.6
│   ├── frida v16.5.2
│   ├── prompt-toolkit v3.0.52
│   │   └── wcwidth v0.2.14
│   ├── pygments v2.19.2
│   └── websockets v13.1
├── litecli v1.17.0
│   ├── cli-helpers v2.7.0
│   │   ├── configobj v5.0.9
│   │   └── tabulate v0.9.0
│   ├── click v8.3.1
│   ├── configobj v5.0.9
│   ├── llm v0.28
│   │   ├── click v8.3.1
│   │   ├── click-default-group v1.2.4
│   │   │   └── click v8.3.1
│   │   ├── condense-json v0.1.3
│   │   ├── openai v2.15.0
│   │   │   ├── anyio v4.12.1
│   │   │   │   ├── idna v3.11
│   │   │   │   └── typing-extensions v4.15.0
│   │   │   ├── distro v1.9.0
│   │   │   ├── httpx v0.28.1
│   │   │   │   ├── anyio v4.12.1 (*)
│   │   │   │   ├── certifi v2026.1.4
│   │   │   │   ├── httpcore v1.0.9
│   │   │   │   │   ├── certifi v2026.1.4
│   │   │   │   │   └── h11 v0.16.0
│   │   │   │   └── idna v3.11
│   │   │   ├── jiter v0.12.0
│   │   │   ├── pydantic v2.12.5
│   │   │   │   ├── annotated-types v0.7.0
│   │   │   │   ├── pydantic-core v2.41.5
│   │   │   │   │   └── typing-extensions v4.15.0
│   │   │   │   ├── typing-extensions v4.15.0
│   │   │   │   └── typing-inspection v0.4.2
│   │   │   │       └── typing-extensions v4.15.0
│   │   │   ├── sniffio v1.3.1
│   │   │   ├── tqdm v4.67.1
│   │   │   └── typing-extensions v4.15.0
│   │   ├── pip v25.3
│   │   ├── pluggy v1.6.0
│   │   ├── puremagic v1.30
│   │   ├── pydantic v2.12.5 (*)
│   │   ├── python-ulid v3.1.0
│   │   ├── pyyaml v6.0.3
│   │   ├── setuptools v80.9.0
│   │   ├── sqlite-migrate v0.1b0
│   │   │   └── sqlite-utils v3.39
│   │   │       ├── click v8.3.1
│   │   │       ├── click-default-group v1.2.4 (*)
│   │   │       ├── pip v25.3
│   │   │       ├── pluggy v1.6.0
│   │   │       ├── python-dateutil v2.9.0.post0
│   │   │       │   └── six v1.17.0
│   │   │       ├── sqlite-fts4 v1.0.3
│   │   │       └── tabulate v0.9.0
│   │   └── sqlite-utils v3.39 (*)
│   ├── mypy v1.19.1
│   │   ├── librt v0.7.8
│   │   ├── mypy-extensions v1.1.0
│   │   ├── pathspec v1.0.3
│   │   └── typing-extensions v4.15.0
│   ├── pip v25.3
│   ├── prompt-toolkit v3.0.52 (*)
│   ├── pygments v2.19.2
│   ├── setuptools v80.9.0
│   └── sqlparse v0.5.5
├── prompt-toolkit v3.0.52 (*)
├── pygments v2.19.2
├── requests v2.32.5
│   ├── certifi v2026.1.4
│   ├── charset-normalizer v3.4.4
│   ├── idna v3.11
│   └── urllib3 v2.6.3
├── semver v2.13.0
└── tabulate v0.9.0
(*) Package tree already displayed
~~~

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