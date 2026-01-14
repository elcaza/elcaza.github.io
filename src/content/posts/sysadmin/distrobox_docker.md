---
title: Distrobox y Docker
published: 2026-01-13
description: 'Dos formas de usar los contenedores'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/distrobox_docker/portada.jpg'
tags: [Sysadmin, Contenedores]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://distrobox.it/" target="_blank">Distrobox</a>
+ <a href="https://www.docker.com/" target="_blank">Docker</a>

# ¿Qué son?

+ Docker es una plataforma para crear y gestionar contenedores aislados (pensada para aplicaciones)
+ Distrobox es una herramienta que utiliza a Docker (o Podman) para crear contenedores altamente integrados con tu sistema (pensada para usuarios y desarrollo)

# ¿Cuándo usar cada uno?

## Usa Docker si:
+ Docker prefiere imágenes inmutables. Si quieres cambiar algo, modificas un Dockerfile y reconstruyes la imagen.
+ Quieres desplegar una aplicación en un servidor.
+ Necesitas aislar totalmente un proceso por seguridad.
+ Estás trabajando con microservicios.

## Usa Distrobox si:
+ Distrobox fomenta contenedores mutables. Entras al contenedor y usas el gestor de paquetes de esa distribución (dnf, pacman, apt) como si fuera una máquina virtual, pero sin el consumo de recursos de una VM.
+ Usas una distro "Immutable" (como Fedora Silverblue o SteamOS) y necesitas instalar herramientas de desarrollo.
+ Necesitas un programa que solo está disponible en los repositorios de otra distribución.
+ Quieres probar software nuevo sin "ensuciar" tu sistema base con dependencias.

## Tabla comparativa - Objetivos
<table>
  <caption>Tabla comparativa: Distrobox vs. Docker</caption>
  <thead>
    <tr>
      <th>Característica</th>
      <th>Docker</th>
      <th>Distrobox</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Objetivo Principal</strong></td>
      <td>Aislar aplicaciones y microservicios.</td>
      <td>Ejecutar herramientas de otras distros en tu PC.</td>
    </tr>
    <tr>
      <td><strong>Aislamiento</strong></td>
      <td>Alto (separa red, archivos y procesos).</td>
      <td>Bajo (comparte tu carpeta personal, audio y GPU).</td>
    </tr>
    <tr>
      <td><strong>Persistencia</strong></td>
      <td>Los cambios se pierden si no usas volúmenes.</td>
      <td>Persistente por defecto (puedes usar <code>sudo apt/pacman</code>).</td>
    </tr>
    <tr>
      <td><strong>Interfaz</strong></td>
      <td>Principalmente línea de comandos/servicios.</td>
      <td>Integración total con aplicaciones gráficas (GUI).</td>
    </tr>
    <tr>
      <td><strong>Arquitectura</strong></td>
      <td>Es un motor de contenedores (Runtime).</td>
      <td>Es un "wrapper" que usa Docker o Podman.</td>
    </tr>
  </tbody>
</table>

## Tabla comparativa - Consumo de recursos

<table>
  <caption>Comparativa de consumo de recursos: Docker vs. Distrobox</caption>
  <thead>
    <tr>
      <th>Recurso</th>
      <th>Docker (Contenedores Estándar)</th>
      <th>Distrobox</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Espacio Base (Imagen)</strong></td>
      <td>Mínimo a Moderado. Alpine (5 MB) o Ubuntu (75 MB).</td>
      <td>Moderado a Alto. Arch Linux puede llegar a 400 MB.</td>
    </tr>
    <tr>
      <td><strong>Persistencia de Datos</strong></td>
      <td>Efímero por defecto. Los cambios se pierden sin volúmenes.</td>
      <td>Permanente por defecto. Los programas instalados se quedan en disco.</td>
    </tr>
    <tr>
      <td><strong>Ubicación en Disco</strong></td>
      <td>Generalmente en /var/lib/docker (requiere root).</td>
      <td>Carpeta de usuario (~/.local/share/containers/storage).</td>
    </tr>
    <tr>
      <td><strong>Memoria RAM</strong></td>
      <td>Variable. Depende del aislamiento de red y procesos.</td>
      <td>Mínimo. Casi idéntico a una aplicación nativa.</td>
    </tr>
    <tr>
      <td><strong>Procesamiento (CPU)</strong></td>
      <td>Casi nativo (ligero overhead por aislamiento).</td>
      <td>Nativo. Sin capas intermedias.</td>
    </tr>
    <tr>
      <td><strong>Gráficos (GPU)</strong></td>
      <td>Complejo. Requiere configuración manual de drivers.</td>
      <td>Transparente. Comparte la GPU del host automáticamente.</td>
    </tr>
    <tr>
      <td><strong>Entrada/Salida (I/O)</strong></td>
      <td>Ligeramente lento por el sistema de archivos por capas.</td>
      <td>Velocidad nativa al trabajar sobre el Home del usuario.</td>
    </tr>
  </tbody>
</table>

## Ventajas y desventajas
+ Distrobox
    + Funciona. Para instalar una aplicación que no corre nativamente en tu sistema. Por ejemplo, Debian a menudo usa librerías muy estables pero no tan actualizadas, entonces aplicaciones como Spotify pueden no correr por falta de esa librería y, actualizarla no es una opción completamente segura.
    + Requieres que se comparta todo tu sistema (configuraciones, archivos, $HOME, pantalla, llaves ssh, étc), en otras palabras, confías en lo que vivirá dentro del contenedor
+ Docker
    + Requieres un entorno aislado. Podrías no confiar en lo que pase dentro del contenedor (por ejemplo scripts con POCs de seguridad)
    + No te importa no tener una integración tan sencilla con el SO

# Instalación de Docker
+ <a href="https://docs.docker.com/engine/install/debian/" target="_blank">Docker install Debian</a>

## Remover docker
~~~bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-doc podman-docker containerd runc | cut -f1)
~~~

## Instalar vía APT
~~~bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
~~~

## Instalar Docker
~~~bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
~~~

## Prueba
~~~bash
sudo docker run hello-world
~~~

# Instalación Distrobox

## Instala podman y distrobox

~~~bash
sudo apt install podman distrobox -y
~~~

## Ejemplo Spotify
+ <a href="https://elcaza.github.io/posts/sysadmin/spotify_distrobox/" target="_blank">Ejemplo Spotify</a>

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