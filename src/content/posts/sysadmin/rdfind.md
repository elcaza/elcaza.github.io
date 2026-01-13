---
title: Borrar archivos duplicados en Linux con rdfind
published: 2025-12-24
description: 'rdfind es una herramienta que te ayuda a eliminar tus archivos duplicados'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/rdfind/portada.jpg'
tags: [sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://rdfind.pauldreik.se/" target="_blank">Sitio web oficial</a>
+ <a href="https://github.com/pauldreik/rdfind" target="_blank">Repositorio de github</a>

# ¿Qué es rdfind?

Rdfind (Redundant Data Find) es una herramienta para encontrar archivos duplicados en tu sistema operativo. rdfind analiza el contenido real de los archivos para determinar si son idénticos, lo que la hace extremadamente confiable para liberar espacio en disco.

## ¿Cómo funciona?
1. Tamaño del archivo: Compara el tamaño de los archivos
1. Primeros bytes: Compara el inicio de los archivos
1. Últimos bytes: Compara el final de los archivos
1. Checksum (Hash): Si todo lo anterior coincide, calcula una firma digital (como MD5 o SHA1) de todo el contenido para confirmar que son idénticos
1. Genera un reporte: Crea un archivo llamado results.txt con la lista de los duplicados encontrados.

## Opciones para procesar los resultados
+ Borrar: Eliminar los archivos redundantes de forma automática.
+ Hardlinks: Reemplazar los duplicados con links físicos (hard links). Esto ahorra espacio pero mantiene el archivo accesible desde múltiples rutas.
+ Symlinks: Reemplazar los duplicados con links simbólicos.

# ¿Cómo instalar?

~~~bash
sudo apt install rdfind
~~~

# Ejecución

~~~bash
rdfind folder
~~~
+ Creará el archivo `results.txt`

# Modo seguro, dryrun
~~~bash
rdfind -dryrun true folder
~~~
+ Te muestra lo que haría con los archivos, es decir, te muestra cuáles estaría borrando sin borrarlos.

# Borrar duplicados
~~~bash
rdfind -deleteduplicates true folder
~~~
+ Se elimina directamente, NO va a la papelera de reciclaje

# Crea hardlinks
~~~bash
rdfind -makehardlinks true folder
~~~
+ Un hardlink hace que dos o más nombres de archivo apunten al mismo contenido físico en el disco (el mismo inode)
+ Resultado: Si tenías dos archivos de 1GB que eran iguales, tras este comando seguirás viendo dos archivos, peroen el disco solo ocuparán 1GB en total. Si borras uno, el otro sigue funcionando perfectamente.

# Crea syslinks
~~~bash
rdfind -makesymlinks true folder
~~~
+ El archivo duplicado se convierte en un pequeño puntero que indica dónde está el archivo original
+ Resultado: El archivo duplicado ahora es solo un "alias". Si borras el archivo original, el enlace simbólico quedará "roto" y no podrás acceder al contenido desde esa ruta

# Cosas a considerar, ¿Cuándo podría fallar?
1. Permisos de lectura: Si hay archivos que no puede leer, no serán analizados
1. Sistemas de archivos diferentes: Si intentas crear hardlinks entre dos discos diferentes (por ejemplo, de un disco duro externo a tu disco interno)
1. Metadatos (Atributos del archivo): Rdfind compara el contenido del archivo, no sus metadatos.
1. El "Fallo Humano": El orden de los argumentos: Si un archivo está en ambas, el de `carpeta_A` se considera el original y el de `carpeta_B` el duplicado. Si te equivocas en el orden, podrías terminar borrando el archivo de la carpeta que querías mantener limpia.
    + Por ejemplo, archivos de github o archivos de máquinas virtuales
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