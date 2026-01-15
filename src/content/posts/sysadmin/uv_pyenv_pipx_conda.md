---
title: uv vs pyenv vs pipx vs conda vs python3 -m venv
published: 2026-01-03
description: 'Un montón de opciones para hacer las mismas cosas'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/uv_pyenv_pipx_conda/portada.jpg'
tags: [Sysadmin, Python]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Tabla comparativa

<table>
    <thead>
        <tr>
            <th>Herramienta</th>
            <th>Función Principal</th>
            <th>Gestiona Versiones Python</th>
            <th>Gestiona Paquetes No-Python</th>
            <th>Velocidad</th>
            <th>Uso Recomendado</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>python3 -m venv</td>
            <td>Aislamiento básico</td>
            <td>No (usa la del sistema)</td>
            <td>No</td>
            <td>Lenta</td>
            <td>Sistemas restringidos o scripts simples.</td>
        </tr>
        <tr>
            <td>pyenv</td>
            <td>Gestión de versiones</td>
            <td>Sí</td>
            <td>No</td>
            <td>N/A</td>
            <td>Cambiar versiones de Python de forma global.</td>
        </tr>
        <tr>
            <td>pipx</td>
            <td>Instalación de Apps</td>
            <td>No</td>
            <td>No</td>
            <td>Media</td>
            <td>Instalar herramientas CLI </td>
        </tr>
        <tr>
            <td>conda</td>
            <td>Entornos científicos</td>
            <td>Sí</td>
            <td>Sí</td>
            <td>Muy Lenta</td>
            <td>Ciencia de datos (Legacy/Tradicional).</td>
        </tr>
        <tr>
            <td>uv</td>
            <td>Todo en uno (Python)</td>
            <td>Sí</td>
            <td>No</td>
            <td>Extrema (Rust)</td>
            <td>Desarrollo de software moderno y rápido.</td>
        </tr>
        <tr>
            <td>pixi</td>
            <td>Todo en uno (Multi-lenguaje)</td>
            <td>Sí</td>
            <td>Sí</td>
            <td>Alta (Rust)</td>
            <td>Proyectos con dependencias C++, CUDA o R.</td>
        </tr>
    </tbody>
</table>

## Ventajas
+ Aislamiento de dependencias: Evitan conflictos entre las bibliotecas. 
+ Limpieza del sistema: Los paquetes se instalan localmente en la carpeta del entorno, manteniendo limpia la instalación global de Python.
+ Portabilidad y replicabilidad: Facilitan compartir tu proyecto. Puedes generar un archivo de requisitos (`requirements.txt`) para que otros desarrolladores puedan replicar exactamente el mismo entorno de trabajo.


# 1. UV
Esta es la opción recomendada

## Instalación de UV

~~~bash
curl -LsSf https://astral.sh/uv/install.sh | sh

    # source $HOME/.local/bin/env (sh, bash, zsh)
    # source $HOME/.local/bin/env.fish (fish)
~~~

### Ejemplo Frida y Objection
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

## Saber que hay instalado en el entorno virtual
~~~bash
# Desde la carpeta en que vive el entorno
uv pip tree
~~~

## Actualizaciones
~~~bash
uv self update
~~~

## Correr en un entorno temporal
+ Por ejemplo un script de Frida

~~~bash
VERSION=16.5.2
uv run --with frida==16.5.2 --with frida-tools --with objection==1.11.0 frida -U -f com.app.app -l script.js
~~~

# 2. Python Virtual Environment (venv)

## (MEJOR USA UV) Instalación de python3-venv
~~~bash
# sudo apt install python3-venv python3-pip -y
~~~

## (MEJOR USA UV) Ejemplo Frida y Objection
~~~bash
# Creamos una carpeta para guardar el entorno
# mkdir frida_16.5.2_venv && cd frida_16.5.2_venv

# Iniciamos el entorno
# python -m venv .venv

# Activamos el entorno
# source .venv/bin/activate

# Instalamos las herramientas que necesitamos
# pip install frida==16.5.2 frida-tools objection==1.11.0

# Corroboramos que todo funcione 
# frida --version
# 16.5.2

# (MEJOR USA UV) Ejemplo un script
# frida -U -f com.app.app -l script.js

# Salimos de nuestro entorno
# deactivate
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