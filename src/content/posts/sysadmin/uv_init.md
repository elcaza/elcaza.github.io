---
title: UV Init - Proyectos y flujos de trabajo
published: 2026-10-29
description: 'Gestiona proyectos de Python con uv init, uv add y uv sync: el flujo nativo de uv'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/uv/portada.jpg'
tags: [Sysadmin, Python]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Introducción

En el <a href="https://elcaza.github.io/posts/sysadmin/uv/" target="_blank">UV Cheatsheet</a> usamos uv a través de su interfaz `uv pip`, que funciona como un pip mucho más rápido. Pero uv tiene un modo de trabajo propio: **el modo proyecto**.

En este modo ya no instalas paquetes "a mano" en un entorno. Declaras las dependencias en un archivo `pyproject.toml` y uv se encarga del resto: crea el entorno, instala lo necesario y guarda las versiones exactas en un archivo de bloqueo (`uv.lock`).

## ¿Por qué usar el modo proyecto?
+ **Sin activar entornos:** `uv run` sincroniza y ejecuta en un solo paso.
+ **Reproducibilidad real:** `uv.lock` guarda la versión exacta de *todas* las dependencias, incluidas las indirectas.
+ **Un solo comando para replicar:** quien clone tu proyecto solo necesita ejecutar `uv sync`.
+ **Gestión de Python incluida:** uv descarga la versión de Python que el proyecto necesite.

## `uv pip` vs modo proyecto

| Tarea | Interfaz `uv pip` | Modo proyecto |
|---|---|---|
| Crear entorno | `uv venv` | `uv init` (el entorno se crea solo) |
| Instalar paquete | `uv pip install paquete` | `uv add paquete` |
| Desinstalar | `uv pip uninstall paquete` | `uv remove paquete` |
| Ver dependencias | `uv pip tree` | `uv tree` |
| Replicar entorno | `uv pip install -r requirements.txt` | `uv sync` |
| Ejecutar | `source .venv/bin/activate` | `uv run comando` |

# 1. Crear un proyecto

## Inicializar

~~~bash
uv init mi_proyecto
cd mi_proyecto
~~~

O, si ya estás dentro de la carpeta:

~~~bash
uv init
~~~

## ¿Qué se crea?

~~~
mi_proyecto/
├── .git/
├── .gitignore
├── .python-version   # Versión de Python del proyecto
├── README.md
├── main.py           # Script de ejemplo
└── pyproject.toml    # Metadatos y dependencias
~~~

El `pyproject.toml` inicial luce así:

~~~toml
[project]
name = "mi_proyecto"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
~~~

:::tip
El `.venv` y el `uv.lock` **no** se crean con `uv init`. Aparecen la primera vez que ejecutas `uv add`, `uv sync` o `uv run`.
:::

## Elegir la versión de Python

~~~bash
# Al crear el proyecto
uv init mi_proyecto --python 3.12

# En un proyecto existente
uv python pin 3.12
~~~

Si no tienes esa versión instalada, uv la descarga automáticamente.

# 2. Gestionar dependencias

## Añadir paquetes

~~~bash
uv add requests

# Con versión específica
uv add requests==2.32.3

# Con un rango de versiones
uv add "requests>=2.31,<3"
~~~

Cada `uv add` actualiza tres cosas a la vez: el `pyproject.toml`, el `uv.lock` y el `.venv`.

## Dependencias de desarrollo

Paquetes que solo necesitas mientras desarrollas (tests, linters, etc.):

~~~bash
uv add --dev pytest ruff
~~~

## Eliminar paquetes

~~~bash
uv remove requests
~~~

## Ver el árbol de dependencias

~~~bash
uv tree
~~~

# 3. Ejecutar código

## Con `uv run`

~~~bash
uv run main.py
uv run python -c "import requests; print(requests.__version__)"
uv run pytest
~~~

Antes de ejecutar, `uv run` comprueba que el entorno esté sincronizado con el `uv.lock`. Si falta algo, lo instala. Por eso no hace falta activar nada.

## Activando el entorno (si lo prefieres)

El modo proyecto sigue usando un `.venv` normal, así que el alias del post anterior sigue funcionando:

### Generar alias
~~~bash
echo "alias uvs='source .venv/bin/activate'" >> ~/.bashrc
~~~

### Activar el entorno en modo manual

~~~bash
source .venv/bin/activate
~~~

# 4. Replicar un proyecto

Este es el punto fuerte del modo proyecto. Si alguien clona tu repositorio:

~~~bash
git clone https://github.com/usuario/mi_proyecto.git
cd mi_proyecto
uv sync
~~~

Listo: misma versión de Python y mismas versiones exactas de cada paquete.

:::important
Sube el `uv.lock` a tu repositorio. Es lo que garantiza que todos tengan exactamente el mismo entorno. El `.venv`, en cambio, no se sube (ya viene en el `.gitignore`).
:::

## Instalar sin dependencias de desarrollo

Útil para servidores o contenedores:

~~~bash
uv sync --no-dev
~~~

## Fallar si el lock no está al día

Útil en CI, para asegurarte de que nadie olvidó actualizar el `uv.lock`:

~~~bash
uv sync --locked
~~~

# 5. Actualizar dependencias

~~~bash
# Actualizar todas las dependencias (respetando los rangos del pyproject.toml)
uv lock --upgrade

# Actualizar solo un paquete
uv lock --upgrade-package requests

# Aplicar los cambios al entorno
uv sync
~~~

# 6. Migrar desde requirements.txt

## Importar

~~~bash
uv init
uv add -r requirements.txt
~~~

## Exportar

Si alguien necesita un `requirements.txt` (por ejemplo, una herramienta que no soporta uv):

~~~bash
uv export --format requirements-txt > requirements.txt
~~~

# Ejemplos

## Ejemplo Frida y Objection

El mismo ejemplo del <a href="https://elcaza.github.io/posts/sysadmin/uv/" target="_blank">UV Cheatsheet</a>, ahora en modo proyecto:

~~~bash
# Creamos el proyecto
uv init 16.5.2 && cd 16.5.2

# Añadimos las herramientas que necesitamos
uv add frida==16.5.2 frida-tools objection==1.11.0

# Corroboramos que todo funcione
uv run frida --version
# 16.5.2

# Ejemplo con un script
# uv run frida -U -f com.app.app -l script.js
~~~

:::tip
Al fijar `frida==16.5.2`, uv elige automáticamente una versión de `frida-tools` compatible con ella. Con pip, a veces terminas con una versión de `frida-tools` que pide otra versión de `frida`.
:::

## Ejemplo xcat con una versión específica de Python

~~~bash
# Creamos el proyecto con Python 3.10
uv init xcat_uv --python 3.10 && cd xcat_uv

# Añadimos xcat
uv add xcat

# Corroboramos que todo funcione
uv run xcat --version
~~~

:::note
Si solo quieres *usar* una herramienta de línea de comandos (como xcat) y no vas a escribir código alrededor de ella, `uv tool` es más cómodo:

~~~bash
uv tool install xcat --python 3.10
xcat --version
~~~
:::

# Bonus: scripts sueltos con dependencias

¿Un script de un solo archivo que necesita librerías? No hace falta crear un proyecto. uv puede guardar las dependencias dentro del propio script:

~~~bash
uv init --script scan.py --python 3.12
uv add --script scan.py requests rich
uv run scan.py
~~~

uv añade un bloque como este al inicio del archivo:

~~~python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "requests",
#     "rich",
# ]
# ///
~~~

Puedes compartir solo el `.py` y quien lo reciba lo ejecuta con `uv run scan.py`, sin instalar nada a mano.

# Cheatsheet rápido

~~~bash
uv init [nombre]              # Crear proyecto
uv init --python 3.12         # Crear proyecto con versión de Python
uv python pin 3.12            # Cambiar la versión de Python
uv add paquete                # Añadir dependencia
uv add --dev paquete          # Añadir dependencia de desarrollo
uv remove paquete             # Eliminar dependencia
uv tree                       # Ver árbol de dependencias
uv run comando                # Ejecutar dentro del entorno
uv sync                       # Sincronizar entorno con uv.lock
uv sync --no-dev              # Sincronizar sin dependencias de desarrollo
uv lock --upgrade             # Actualizar todas las dependencias
uv lock --upgrade-package p   # Actualizar un paquete
uv add -r requirements.txt    # Importar requirements.txt
uv export --format requirements-txt > requirements.txt  # Exportar
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