---
title: pyenv vs pipx vs conda
published: 2025-06-21
description: 'Entornos virtuales en python.'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/pyenv_pipx_conda/portada.jpg'
tags: [Sysadmin, Python]
category: 'Sysadmin'
draft: false 
lang: 'es'
---
Pendiente
# Entornos virtuales en python
Los entornos virtuales en Python son espacios aislados que contienen su propio intérprete de Python y un conjunto de paquetes o dependencias instaladas, independientes de la instalación global de Python en tu sistema.

## Ventajas
+ Aislamiento de dependencias: Evitan conflictos entre las bibliotecas. 
+ Limpieza del sistema: Los paquetes se instalan localmente en la carpeta del entorno, manteniendo limpia la instalación global de Python.
+ Portabilidad y replicabilidad: Facilitan compartir tu proyecto. Puedes generar un archivo de requisitos (`requirements.txt`) para que otros desarrolladores puedan replicar exactamente el mismo entorno de trabajo.

## 

# Python Virtual Environment (venv)
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

# Python Virtual Environment (pipx)
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