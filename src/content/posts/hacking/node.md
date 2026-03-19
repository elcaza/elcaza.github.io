---
title: Laboratorio de pruebas con node y npm
published: 2026-03-01
description: 'Genera diccionarios con Crunch'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/node/portada.jpg'
tags: [Hacking, Linux]
category: 'Hacking'
draft: false 
lang: 'es'
---

# Objetivo

Instalar node a través de Node Version Manager para ejecutar laboratorios vulnerables.

# Instalación 
## Fuente
+ <a href="https://github.com/nvm-sh/nvm" target="_blank">Github</a>

## Opción 1:
~~~bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
~~~

## Opción 2:
~~~bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
~~~

## Corroborar instalación
Abre una nueva terminal y ejecuta
~~~bash
nvm --version
# 0.40.4
~~~

## Listar opciones de instalación
~~~bash
nvm ls-remote --lts
~~~

## Opciones de instalación y uso
~~~bash
# Instala la última estable.
nvm install --lts

# Instala una versión específica.
nvm install 16.20.0 

# Cambia instantáneamente a la versión 18.
nvm use 18 
~~~

# Verificar seguridad del aplicativo antes de iniciar

~~~bash
# Te dirá cuántas vulnerabilidades conocidas hay.
npm audit
~~~

# Corre una aplicación

~~~bash
npm run dev
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