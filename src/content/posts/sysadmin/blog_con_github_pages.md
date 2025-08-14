---
title: Crea tu propio blog con Github Pages
published: 2025-08-13
description: 'Description'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/como_funcionan_los_activadores_kms.webp'
tags: [Sysadmin]
category: 'Sysadmin'
draft: true 
lang: 'es'
---

# Introducción
Para ordenar tus apuntes, para públicar historias o para respaldar tu información siempre es útil un blog.
Sin embargo, hostearlo y hacer que este se encuentre disponible a largo plazo podría ser costoso.
Para resolver esto, en este post veremos la manera de públicar un blog gratis con Github Pages.

Para esto vamos a requerir un par de cosas.

1. Una cuenta de Github
2. Seleccionar un tema compatible con Github Pages
3. Seguir este tutorial paso a paso

# 1. Cuenta de Github
+ Crear una cuenta en Github
+ https://github.com

# 2. Seleccionar un tema compatible con Github Pages
Existen múltiples templates que pueden usarse con Github Pages. A continuación se muestran algunos:
+ https://github.com/guilyx/awesome-github-pages-portfolios?tab=readme-ov-file
+ https://docs.github.com/en/pages
+ https://github.com/saicaca/fuwari?tab=readme-ov-file

En nuestro caso, usaremos el último link correspondiente al tema *Fuwari de Saicaca*

# 3. Instalación del tema Fuwari de Saicaca

## Clonamos el repositorio
+ Ingresamos a: https://github.com/saicaca/fuwari?tab=readme-ov-file
+ Clonamos el repositorio

## Descargamos el repositorio 
+ Se asume que ya tienen acceso a su repositorio por medio de llave pública y privada
	+ https://docs.github.com/es/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
	+ Adjuntar post
+ Descargas tu repositorio

## Instalar las dependencias para el tema
+ Esto debe hacerse dentro de la carpeta del repositorio clonado

En una terminal 

~~~bash
# Actualizas la lista de paquetes disponibles 
sudo apt update

# Instala node
sudo apt install npm -y

# Instala pnpm
sudo npm install -g pnpm

# Dentro del repositorio instala las dependencias
pnpm install
pnpm add sharp
~~~

## Corrobora que el sitio pueda visualizarse de manera local

~~~
# Construye el sitio
pnpm run build

# Previsualiza el sitio 
pnpm preview 
~~~



===============================
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/1.webp" width="100%">


:::tip[Nota final]
¡Gracias por terminar de leer este artículo! uwur

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>