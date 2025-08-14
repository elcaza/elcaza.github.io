---
title: ¿Cómo generar tu ssh key para Github?
published: 2025-08-13
description: 'Description'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/como_funcionan_los_activadores_kms.webp'
tags: [Sysadmin, Linux]
category: 'Sysadmin'
draft: true 
lang: 'es'
---

# Introducción

+ Este post pretende únicamente servir como guía rápida para generar y añadir tu llave ssh key a github.
+ No vamos a profundizar en el cómo funciona ni en la belleza de la criptografía asimétrica.

# 1. Creación de tu ssh key para Github 

+ Esto generará dos llaves:
    + `yourkey` (llave privada - esta **NO se debe compartir**, *piénsalo como la llave de un candado*)
    + `yourkey.pub` (llave pública - esta es la que se debe subir a Github, *piénsalo como el candado*)
    
~~~bash
# Generamos una llave
ssh-keygen -t ed25519 -C "email@gmail.com"   

# Entramos a nuestra carpeta .ssh
cd ~/.ssh

# Obtenemos la información que pegaremos en Github
cat your_key.pub
~~~

**Imágenes del proceso:**

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/1.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/2.png" width="100%">


# 2. Subimos nuestra ssh key a Github

1. Entramos a las <a href="https://github.com/settings/profile" target="_blank">configuracioines de perfil en Github</a>
1. Seleccionamos `Password and authentication`
1. Seleccionamos `SSH and GPG keys`
1. Seleccionamos `New SSH key`
1. Añadimos los datos requeridos y presionamos `Add SSH key`

**Imágenes del proceso:**

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/3.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/4.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/5.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/6.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/7.png" width="100%">

# 3. Probamos que todo funcione de manera correcta 

1. Entramos al <a href="https://github.com/settings/profile" target="_blank">repositorio a clonar</a>
1. Seleccionamos `Code`
1. Seleccionamos `el icono de copiar`
1. Abrimos una terminal y ejecutamos lo siguiente

~~~bash
cd ~/Documents 

# Pegamos lo que has copiado
git clone git@github.com:tu_usuario/tu_repo.git 
~~~

**Imágenes del proceso:**

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/8.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/9.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssh_key_github/10.png" width="100%">

:::tip[Nota final]
¡Gracias por terminar de leer este artículo! uwur

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>