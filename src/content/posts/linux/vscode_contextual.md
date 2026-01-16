---
title: Abre carpetas y documentos con vscode desde el menú contextual de Gnome
published: 2026-01-15
description: 'Añade la opción de abrir con vscode en el menú contextual (click derecho) de Gnome'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/vscode_contextual/portada.jpg'
tags: [Linux]
category: 'Linux'
draft: false 
lang: 'es'
---

# El problema
Para abrir una carpeta en vscode se tiene que
1. Click derecho sobre la parte en blanco de la carpeta
1. Abrir una nueva terminal
1. Escribir `code .`

# La solución
Añadir acciones al menú contextual. Para esto tenemos dos opciones:
1. Scripts de nautilus
1. Extensión oficial

## Scripts de nautilus

### Instalar dependencias
~~~bash
sudo apt install nautilus-extension-gnome-terminal
~~~

### Crear script
~~~bash
cd ~/.local/share/nautilus/scripts
vim vscode
~~~

### Contenido del archivo
~~~bash
#!/bin/bash
# Comprueba si hay una selección, si no, abre la carpeta actual
if [ -n "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS" ]; then
	echo "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS" | xargs -d '\n' code
else
	code "$NAUTILUS_SCRIPT_CURRENT_URI"
fi
~~~

### Dar permiso de ejecución
~~~bash
chmod +x vscode
~~~

### Corrobora
1. Da click derecho sobre una carpeta o un archivo
1. Da click en el menú de scripts
1. Da click en vscode

## Instala dependencias
~~~bash
sudo apt install python3-nautilus
~~~

### Crear script
~~~bash
mkdir -p ~/.local/share/nautilus-python/extensions
cd ~/.local/share/nautilus-python/extensions
vim vscode_extension.py
~~~

### Contenido del script
~~~bash
import os
from gi.repository import Nautilus, GObject

class VSCodeExtension(GObject.GObject, Nautilus.MenuProvider):
		def launch_vscode(self, menu, files):
				paths = []
				for f in files:
						paths.append('"' + f.get_location().get_path() + '"')
				
				full_command = "code " + " ".join(paths) + " &"
				os.system(full_command)

		def get_file_items(self, *args):
				files = args[-1]
				item = Nautilus.MenuItem(
						name='VSCodeOpen',
						label='Abrir en VS Code',
						tip='Abrir selección con Visual Studio Code'
				)
				item.connect('activate', self.launch_vscode, files)
				return [item]

		def get_background_items(self, *args):
				folder = args[-1]
				item = Nautilus.MenuItem(
						name='VSCodeOpenBackground',
						label='Abrir en VS Code',
						tip='Abrir la carpeta actual con Visual Studio Code'
				)
				item.connect('activate', self.launch_vscode, [folder])
				return [item]
~~~

# La conclusión
¿Para qué fácil si se puede difícil? Quizá con el paso de los meses se compense la media hora invertida en este post.

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