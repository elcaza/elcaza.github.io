---
title: Do Recon Raccoon Tool - Una herramienta para realizar el reconocimiento automatizado en pruebas de penetración
published: 2025-03-17
description: 'Una herramienta que te ayuda a analizar la infraestructura. Déjanos hacer la importante, pero tediosa, tarea de mapear la red y realizar el reconocimiento.'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/do_recon/do_recon.jpeg'
tags: [Hacking]
category: 'Hacking'
draft: false 
lang: 'es'
---

La herramienta se puede encontrar en: + <a href="https://github.com/elcaza/do_recon" target="_blank">Github</a>

# Introducción

Cuando realizas pruebas de seguridad informática (Hacking Ético / Pentest), una de las etapas más importantes es el reconocimiento. Este es un pilar, pero puede llegar a ser un tanto aburrido y angustiante cuando tienes múltiples activos por evaluar.

Imagina esto, tienes que realizar pruebas a 100 activos en el menor tiempo posible. Además, toda tu evidencia debe estar documentada por si el cliente tiene dudas acerca del servicio. Por tanto, tus pruebas tienen que contemplar lo siguiente:

1. Todo debe estar correctamente documentado y ordenado en su propia carpeta
1. Debes poder responder qué activos han sido alcanzados y cuáles no
1. Si hubo activos que no se alcanzaron, ¿Cuándo sucedió esto? ¿Estabas conectado a una VPN? ¿Desde qué IP trataste de acceder? ¿Qué ruta se realizo para tratar de alcanzar al activo?
1. Finalmente, te debe sobrar tiempo para realizar tus pruebas y detallar cada hallazgo identificado

Si bien este proceso puede realizarse de forma manual, la mejor forma de abordar este problema es automatizar lo que puedas automatizar y utilizar el tiempo restante en el análisis de la información recabada y el uso de la misma para tratar de vulnerar los sistemas.

# ¿Qué hace la herramienta Do Recon?

Básicamente, por etapas, se encarga de la importante pero tediosa tarea de hacer el reconocimiento de los activos. Te entrega un reporte por cada etapa finalizada. De esta manera te permite avanzar de manera simultanea en las pruebas manuales.

En orden (Tú puedes elegir cuáles de estas opciones ejecutar):

1. **Crea un archivo con logs para todos los activos:** de esta manera podrás identificar qué activos han sido escaneados, cuándo se realizaron y en caso de errores o intermitencia poder corregirlos
1. **Corrobora las interfaces de red disponibles en el sistema:** de esta manera se mantiene la visibilidad de cuáles interfaces fueron utilizadas durante las pruebas de seguridad
1. **Crea una carpeta para cada activo escaneado:** de esta manera tendrás toda la evidencia debidamente organizada
1. **Realiza una búsqueda nslookup:** con esto podrás relacionar el nombre de dominio y las direcciones IPs que resuelven al mismo
1. **Realiza un traceroute:** Te permitirá conocer si el activo se alcanza de forma correcta y, en caso de que no, saber en qué punto no se resuelve el dominio de manera correcta
1. **Realiza una escaneo de nmap limitado a un — —top-ports:** de esta manera se tendrá un rápido análisis inicial de qué servicios pueden ser probados, por lo que puedes comenzar a trabajar en tus pruebas manuales mientras la herramienta recaba más información
1. **Realiza una escaneo de nmap -sV únicamente a los puertos identificados previamente:** al limitar el escaneo a los activos y puertos identificados, este escaneo se procesará de manera eficiente, del mismo modo te brindará un informe más detallado con el cuál podrás seguir haciendo pruebas manuales
1. **Realiza una escaneo de nmap -p- :** Con la información anterior ya puedes realizar múltiples pruebas, sin embargo aún falta realizar un análisis de todos los puertos (este proceso puede demorar bastante). Por lo que, mientras tú te ocupas de tus pruebas manuales, la herramienta realizará este escaneo completo
1. **Realiza una escaneo de nmap -sV -sC únicamente a los puertos identificados previamente:** ahora que se conocen todos los puertos disponibles para el activo, se realiza un escaneo limitado a estos puertos con la opción -sC (scripts comunes) y te brindará un informe aún más detallado con el cuál podrás seguir haciendo pruebas manuales
1. **Realiza una escaneo de nmap -sV — — script vuln únicamente a los puertos identificados previamente:** ahora que se conocen todos los puertos disponibles para el activo, se realiza un escaneo limitado a estos puertos con la opción — — script vuln (scripts para detectar vulnerabilidades en un servicio), este escaneo te brindará un informe aún más detallado con el cuál podrás seguir haciendo pruebas manuales
1. **Realiza reportes de las pruebas:** tras recabar toda la información, la herramienta procede a elaborar tres reportes. 1. Todos los puertos abiertos; 2. Todos los puertos abiertos y el banner del mismo; 3. Reporte con los CVEs identificados.

# ¿Cómo instalar y manejar la herramienta?

## Requisitos de instalación

+ nmap
+ traceroute
+ nslookup

~~~bash
sudo apt update && sudo apt install nmap traceroute dnsutils
~~~

## Descarga e instalación

~~~bash
git clone https://github.com/elcaza/do_recon.git
cd do_recon
sudo cp do_recon.sh /usr/local/bin/
do_recon.sh input_file.txt
~~~

## Desinstalar la herramienta

~~~bash
sudo rm /usr/local/bin/do_recon.sh
~~~

## ¿Cómo correr la herramienta?

~~~bash
do_recon.sh input_file.txt
~~~

## Ejemplo del archivo de entrada

~~~bash
1.site1.com
2.site2.org.com
3.anothersite.com
4.example.com
5.example2.com
6.other.es
~~~

## ¿Cómo parar la herramienta?

~~~bash
./do_recon.sh stop
~~~

## ¿Cómo eliminar todos los archivos y carpetas creadas por la herramienta?

:::warning[Importante]
Sé cuidadoso, pues este comando eliminará todas las carpetas y archivos .log que se encuentren en la ruta actual
:::

~~~bash
./do_recon.sh reset
~~~

# Ejemplo de la estructura de archivos que generará la herramienta

Selección de nuestra imagen ISO de Windows 10 y opción “Connect atpower on”
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/do_recon/1.webp">

# ¿Más información?

Para obtener más información sobre las opciones y configuraciones de la herramienta te recomendamos visitar el repositorio oficial.

::github{repo="elcaza/do_recon"}

Algunas de las opciones que puedes elegir ejecutar son las siguientes:

<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/do_recon/2.webp">

:::tip[Nota final]
¡Gracias por terminar de leer este artículo uwur!

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>