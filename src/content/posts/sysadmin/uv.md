---
title: UV Cheatsheet
published: 2026-03-19
description: 'UV, la mejor alternativa para gestionar entornos virtuales de python'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/uv/portada.jpg'
tags: [Sysadmin, Python]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Ventajas de UV
+ Aislamiento de dependencias: Evitan conflictos entre las bibliotecas. 
+ Limpieza del sistema: Los paquetes se instalan localmente en la carpeta del entorno, manteniendo limpia la instalación global de Python.
+ Portabilidad y replicabilidad: Facilitan compartir tu proyecto. Puedes generar un archivo de requisitos para que otros desarrolladores puedan replicar exactamente el mismo entorno de trabajo.


# 1. UV

## Instalación de UV

~~~bash
curl -LsSf https://astral.sh/uv/install.sh | sh

    # source $HOME/.local/bin/env (sh, bash, zsh)
    # source $HOME/.local/bin/env.fish (fish)
~~~

## Creación de entorno

~~~bash
uv venv --python 3.12
~~~

## Activación del entorno

~~~bash
source .venv/bin/activate
~~~

## Instalación de los paquetes

~~~bash
uv pip install frida==16.5.2 frida-tools objection==1.11.0
~~~

## Desactivar 

~~~bash
deactivate
~~~

## Saber qué hay instalado en el entorno virtual
~~~bash
# Desde la carpeta en que vive el entorno
uv pip tree
~~~

## Actualizaciones
~~~bash
uv self update
~~~

## Ejecución del comando sin necesidad de activar el entorno
1. Te situas en la carpeta donde vive tu `.venv`
1. Inicias la aplicación que requieras

~~~bash
uv run frida-ls-devices
~~~

## Crear alias para hacer el source más rápido
Vamos a crear un alias en el archivo `.bashrc`

### 1. Abrimos el archivo `.bashrc`
~~~bash
vim ~/.bashrc
~~~

### 2. Añadimos al final del archivo nuestro alias
~~~bash
alias uvs='source .venv/bin/activate'
~~~

### 3. Uso
En una nueva terminal ya podremos ver nuestro alias funcional.
1. Desde la carpeta con el entorno virtual
~~~bash
uvs
~~~

Si no quieres iniciar una nueva terminal, solo tenemos que actualizar el source manualmente
~~~bash
source ~/.bashrc
~~~

## Correr en un entorno temporal
+ Por ejemplo un script de Frida

~~~bash
VERSION=16.5.2
uv run --with frida==16.5.2 --with frida-tools --with objection==1.11.0 frida -U -f com.app.app -l script.js
~~~

# Ejemplos

## Ejemplo Frida y Objection
~~~bash
# Creamos una carpeta para guardar el entorno
mkdir frida_16.5.2_uv && cd frida_16.5.2_uv

# Iniciamos el entorno
uv venv

# Activamos el entorno
source .venv/bin/activate

# Instalamos las herramientas que necesitamos
uv pip install frida==16.5.2 frida-tools objection==1.11.0

# Corroboramos que todo funcione 
frida --version
# 16.5.2

# Ejemplo un script
# frida -U -f com.app.app -l script.js

# Salimos de nuestro entorno
deactivate
~~~

### Ejemplo xcat con una versión especifica de Python
~~~bash
# Creamos una carpeta para guardar el entorno
mkdir xcat_uv && cd xcat_uv

# Iniciamos el entorno
uv venv --python 3.10

# Activamos el entorno
source .venv/bin/activate

# Instalamos las herramientas que necesitamos
uv pip install xcat

# Corroboramos que todo funcione 
xcat --version

# Salimos de nuestro entorno
deactivate
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