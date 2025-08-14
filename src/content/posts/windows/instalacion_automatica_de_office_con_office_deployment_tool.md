---
title: Instalación automática de Office con Office Deployment Tool
published: 2020-05-18
description: '¿Sabías que es posible instalar cualquier versión de office sin la necesidad de hacer configuraciones paso a paso a través del menú de instalación?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/office_dt/1.webp'
tags: [Windows, Activadores]
category: 'Windows'
draft: false 
lang: 'es'
---

¿Sabías que es posible instalar cualquier versión de office sin la necesidad de hacer configuraciones paso a paso a través del menú de instalación? ¿Sabes por qué el software KMS es capaz de “activar” office con tan solo un simple script? ¿Cuáles son los riesgos de hacer esto?

Este artículo constará de dos partes:

1. 1ra. Instalación automática de Office con Office Deployment Tool.
1. 2da. Explicación sobre por qué funciona la activación mediante el script KMS (<a href="https://elcaza.github.io/blog/posts/windows/como_funcionan_los_activadores_kms/" target="_blank">¿Cómo funcionan los activadores KMS?)</a>

:::warning[Advertencia]
1. Este material es meramente educativo y no se incentiva de ninguna manera a el uso de la piratería. Solamente usted es responsable por sus acciones.
1. Como alternativa recomendamos: La compra de licencias originales, Obtener licencias gratuitas para estudiantes, usar software libre.
1. Puedes consultar el código utilizado a través del repositorio de <a href="https://github.com/elcaza/office_autoinstall_explained" target="_blank">Github</a> 
1. Puedes ver el vídeo de este post en <a href="https://youtu.be/18rJjlYeEFk" target="_blank">youtube</a>
:::

# 1. Instalación Automatizada de Office

La mayor parte de las aplicaciones para windows se pueden configurar a través de archivos **.xml**. Para el caso de office contamos con la herramienta <a href="https://docs.microsoft.com/en-us/deployoffice/overview-office-deployment-tool" target="_blank">Office Deployment Tool</a> que nos permitirá introducir un archivo de configuración .xml para lograr una instalación automatizada.

Para realizar este procedimiento lo primero que debes realizar es descargar los archivos requeridos de <a href="https://github.com/elcaza/office_autoinstall_and_hack_explained/archive/master.zip" target="_blank">el repositorio de github</a>. Una vez descargados y descomprimidos los archivos, nos encontraremos con la siguiente estructura de directorios.

:::tip[Tip]
+ Si solamente te interesa desplegar la instalación puedes saltar hasta la sección 1.3 
+ Sin embargo, si te gusta la carnita del cómo funcionan las cosas...
+ ¡Quédate que te lo explico paso a paso!
:::

## Contenido de la carpeta deploy_office

Todo lo necesario para lanzar la instalación de office de una manera automatizada, se encuentra dentro de la carpeta `deploy_office`.

Contenido de la carpeta deploy_office

<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/office_dt/1.webp">

## 1.1 ¿Cómo se configura el archivo .xml?

Nosotros ya contamos con diversos archivos de configuración `.xml` por defecto. Sin embargo, estos pueden ser modificados para que se acoplen completamente a nuestras necesidades. A continuación explicaremos las partes del archivo `config_sample.xml`.

~~~xml
<Configuration>
<Add OfficeClientEdition="64" Channel="Monthly">
    <Product ID="O365ProPlusRetail">
      <Language ID="es-es" />
      <ExcludeApp ID="Access" />
      <ExcludeApp ID="InfoPath" />
      <ExcludeApp ID="Lync" />
      <ExcludeApp ID="SharePointDesigner" />
      <ExcludeApp ID="Visio" />
      <ExcludeApp ID="Skype" />
      <ExcludeApp ID="Skypeforbusiness" />
      <ExcludeApp ID="Groove" />
    </Product>
    <Product ID="VisioProRetail">
      <Language ID="es-es" />
    </Product>
  </Add>
</Configuration>
~~~

Como podemos observar el archivo de configuración únicamente consta de un par de etiquetas en las que destacan:

### Add OfficeClientEdition

Aquí podemos elegir los valores `32` o `64` dependiendo si queremos hacer la instalación de 32bits o 64bits

### Product ID
+ `O365ProPlusRetail` (Office 365 ProPlus)
+ `ProPlusRetail` (Office 2016 ProPlus)
+ `en-us` (Language I)
+ <a href="https://docs.microsoft.com/en-us/deployoffice/overview-deploying-languages-microsoft-365-apps" target="_blank">Puede ver todos los códigos de lenguajes aquí</a>
+ ExcludeApp ID (Aplicaciones a excluir. No serán instaladas)
+ Product ID (Aplicaciones extras que se incluirán)

Aquí podemos elegir los valores `32` o `64` dependiendo si queremos hacer la instalación de 32bits o 64bits

## 1.2 ¿Cómo se configura el archivo .cmd?

Ya que nuestro `setup.exe` depende de un archivo de configuración, este debe correrse a través de la consola, ya sea mediante `cmd` ó `powershell`. Para facilitar esto nosotros podemos crear un archivo `install_config.cmd`.

+ Nuevamente, nosotros ya tenemos un par de archivos preconfigurados.

### ¿Qué hay dentro de este archivo?

~~~bat
@echo off
cd /d %~dp0
setup.exe /configure config_sample.xml
pause
~~~

Básicamente este pequeño script lo que realiza es:

1. Llamar a `setup.exe`
1. Dar la bandera `/configure` que indica que le darás un archivo de configuración
1. Indicar el nombre del archivo de configuración `install_config.cmd`

Si lo queremos modificar solamente cambiamos el punto *“número 3”* especificando el nombre del archivo de configuración que se tendrá.
+ Si creamos un archivo desde cero, hay que recordar guardarlo con extensión **.bat** o **.cmd**

## 1.3 ¿Cómo comenzar la instalación automatizada?

Para comenzar una instalación automatizada únicamente debemos dirigirnos hacia la carpeta `deploy_office` y hacer doble click en el `install_config.cmd` deseado.

Como vimos anteriormente contamos con distintas configuraciones dependiendo de nuestras necesidades

+ Office365_x64
+ Office365_x32
+ Una customizada, en caso de que la haya creado

Una vez ejecutado nuestro `install` aparecerá la típica pantalla de instalación de Office y tras varios minutos se habrá completado la instalación. (El tiempo que tarde dependerá principalmente de su velocidad de descarga de internet).

Una vez que eso haya finalizado tendremos disponible nuestro Office. Sin embargo, este requiere ser activado posteriormente. Para esto, hay dos formas básicas de realizarlo.

1. Compra de licencia de Microsoft Office
1. Activador KMS del cual se habla en <a href="https://elcaza.github.io/blog/windows/posts/windows/como_funcionan_los_activadores_kms/">¿Cómo funcionan los activadores KMS? </a>
    + Ventajas y desventajas
    + ¿Es algo seguro?
    + ¿Por qué funciona?

### Vídeo sobre este artículo
<a href="https://youtu.be/18rJjlYeEFk" target="_blank">Youtube</a>

:::tip[Nota final]
¡Gracias por terminar de leer este artículo!

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>