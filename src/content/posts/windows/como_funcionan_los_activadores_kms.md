---
title: ¿Cómo funcionan los activadores KMS?
published: 2020-08-13
description: '¿Cómo funcionan los activadores KMS?  ¿Sabes por qué el software KMS es capaz de “activar” office con tan solo un simple script?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/como_funcionan_los_activadores_kms.webp'
tags: [Windows, Activadores]
category: 'Windows'
draft: false 
lang: 'es'
---

A menudo la alternativa más utilizada para piratear productos de Microsoft (Windows y Office) es utilizar los activadores KMS, de estos existen diversos *archivos.exe* entre los cuales destacan:

+ KMSpico
+ KMS Tools
+ KMSAuto Net
+ Microsoft Toolkit

Sin embargo, estos finalmente son archivos ejecutables de los cuáles no conocemos con exactitud las acciones que realizan. Ahora, si existen estos activadores, también debemos poder realizar la activación de forma “casi manual”. Es decir, analizar el código fuente de estos activadores.

Tras una búsqueda en internet llegamos a los scripts proporcionados por <a href="http://msguides.com/" target="_blank">msguides</a> y <a href="https://info.gwdg.de/dokuwiki/doku.php?id=en%3Aservices%3Ageneral_services%3Asoftware_and_license_management%3Amskms#manual_kms_activation" target="_blank">gwdg</a>, los cuales nos permiten realizar la activación de los productos de Microsoft de una manera “Casi manual”. A continuación analizaremos cómo funciona y por qué funciona los scripts.

**ADVERTENCIA**:

+ Este material es meramente educativo y no se incentiva de ninguna manera a el uso de la piratería. Solamente usted es responsable de sus acciones.
+ Puedes consultar el código utilizado a través del <a href="https://github.com/elcaza/activadores_kms_explicados" target="_blank">repositorio de Github.</a>

# ¿Qué es el servicio KMS?

El Servicio de Administración de Claves KMS (Key Management Service) es un servicio legitimo de Windows y está diseñado para que las organizaciones activen sus productos por volumen.

Es decir, la organización contrata un paquete de licencias que serán utilizadas por toda su empresa donde, a diferencia del licenciamiento estándar de Microsoft, estas no se anclan a una máquina en especifico sino a la cuenta de una empresa.

Por lo anterior, para este tipo de activación se debe contemplar lo siguiente:

1. La empresa debe montar su propio servidor KMS.
1. Requieren un mínimo de activaciones para funcionar. Es decir, para hacer válidas las activaciones varia el número MÍNIMO de clientes que utilicen la licencia. De otro modo las licencias no serán validadas.
*Por ejemplo*, si el paquete de office requiere un mínimo de 10 usuarios para realizar la activación por lote, tu servidor KMS debe tener registradas al menos a 10 máquinas para que todas cuenten con la licencia por lote de Office, si en algún momento bajan a solamente 9 activaciones todas las máquinas dejarán de tener una licencia válida de Office.
1. La activación por lote solamente se activa de manera temporal, durante 180 días, y su licencia debe ser renovada antes de que termine ese plazo. Sin embargo, este proceso lo hace automáticamente el software por lo que mientras el servidor KMS se mantenga online y activo no habrá problema.

# ¿Cómo funcionan los activadores KMS?

Gracias a la existencia del servicio KMS (Key Management Service) es posible que cualquier persona pueda montar su propio servidor KMS modificado para que este apruebe las licencias de los usuarios.

Para esto solamente se requiere lo siguiente:

1. Montar tu propio servidor KMS
1. Hacer que este tenga una licencia “válida”
1. Hacer que tus computadoras consulten a TU SERVIDOR KMS

Para esto ya existe un script proporcionado por <a href="http://msguides.com/" target="_blank">msguides</a> el cual se conecta a su propio Servidor KMS. Es decir, msguides ya hizo todo el trabajo.

# ¿Cómo se valida la licencia de Windows mediante un script KMS?

Para validar una licencia de Windows mediante el servidor KMS que proporciona <a href="http://msguides.com/" target="_blank">msguides</a> tenemos que realizar lo siguiente:

1. Ubicar que versión de Windows tenemos instalada
1. Cambiar nuestra clave de producto por una valida para el servidor KMS
1. Cambiar nuestro servidor de activación por uno proporcionado por msguides
1. Activar el producto

## 1. Verifica tu versión de Windows instalada

Desde nuestro explorador de archivos, daremos click derecho sobre *“This PC”* y seleccionaremos *“Properties”*. Encontraremos entonces una ventana como la siguiente:

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/1.webp" width="100%">

## 2. Cambiar nuestra clave de producto por una válida para el servidor KMS

Una vez que conocemos la versión de Windows que tenemos procedemos a ubicar el número de clave que tenemos que utilizar.

~~~markdown
Win 10 Home: TX9XD-98N7V-6WMQ6-BX7FG-H8Q99 
Win 10 Home N: 3KHY7-WNT83-DGQKR-F7HPR-844BM 
Win 10 Home Single Language: 7HNRX-D7KGG-3K4RQ-4WPJ4-YTDFH 
Win 10 Home Country Specific: PVMJN-6DFY6-9CCP6-7BKTT-D3WVR 
Win 10 Professional: W269N-WFGWX-YVC9B-4J6C9-T83GX 
Win 10 Professional N: MH37W-N47XK-V7XM9-C7227-GCQG9 
Win 10 Education: NW6C2-QMPVW-D7KKK-3GKT6-VCFB2 
Win 10 Education N: 2WH4N-8QGBV-H22JP-CT43Q-MDWWJ 
Win 10 Enterprise: NPPR9-FWDCX-D2C8J-H872K-2YT43 
Win 10 Enterprise N: DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4   
~~~

+ Para consultar más claves puedes revisar <a href="https://docs.microsoft.com/es-es/windows-server/get-started/kmsclientkeys#span-data-ttu-id58ca7-277versiones-anteriores-de-windowsspanspan-classsxs-lookupspan-data-stu-id58ca7-277earlier-versions-of-windowsspanspan"  target="_blank">la configuración del cliente KMS</a>.


Para nuestro caso utilizaremos la versión de Windows 10 Professional. Por lo que desde una pestaña cmd con derechos de Administrador teclearemos lo siguiente:

~~~txt
slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX 
~~~

+ **/ipk** establece una clave de producto

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/2.webp" width="100%">

## 3. Cambiar nuestro servidor de activación por uno proporcionado por msguides

~~~
slmgr /skms kms8.msguides.com
~~~

+ **/skms** establece el nombre del servidor kms
+ Si por alguna razón da un error `kms8.msguides.com` se puede probar con: `kms7.msguides.com` ó `kms9.msguides.com`

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/3.webp" width="100%">

## 4. Activar el producto

~~~
slmgr /ato
~~~

+ **/ato** realiza la activación

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/4.webp" width="100%">

Si volvemos a las propiedades del sistema del paso número 1 podremos ver que ahora nuestro windows ya está activado

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/5.webp" width="100%">

:::note[Nota]
En caso de que la activación no funcione se deberá utilizar el siguientes comando y reintentar nuevamente el proceso desde el paso 1
:::

~~~
slmgr -upk
~~~

+ **/upk** Desinstala la clave actual de Windows.

# ¿Cómo se valida la licencia de Office mediante un script KMS?

Para utilizar este script es necesario descomprimir el archivo `activate_office.zip` desde <a href="https://github.com/elcaza/activadores_kms_explicados" target="_blank">el repositorio de Github</a>. Este archivo está protegido por contraseña porque de otro modo Windows Defender lo detecta como amenaza al descargarlo.

+ Asegúrese de que Windows Defender esté desactivado.

:::important[Password]
~~~
activate
~~~
:::

Una vez descomprimido el archivo solamente tenemos que dirigirnos a la carpeta **activate_office** y *ejecutar como administrador* el archivo **activate.cmd**

1. Click derecho, ejecutar como administrador
1. Nos abrirá una consola
1. Presionaremos enter una vez. Esto tardará un poco, en caso de ser necesario presionaremos enter de nuevo.
1. Debe aparecernos: `<Product activation successful>` *(De no ser así es probable que no haya corrido el script con privilegios de administrador)*
1. Luego de ello nos preguntará si queremos visitar el blog del autor de este script
1. Y así habremos comprobado que efectivamente el office ha quedado activado

:::note[Dato curioso]
Tu office se transforma a Office 2016 Pro Plus debido a que no tiene licenciamientos disponibles para las otras versiones. Sin embargo prevalecen las funciones.
:::

# ¿Es seguro utilizar activadores KMS?

Como vimos anteriormente el problema no es usar activadores KMS, pues estos son proporcionados por Microsoft a las empresas que pagan licenciamiento por volumen. El verdadero problema es llegar a la piratería de estos servicios, pues es ahí donde nos podemos encontrar con software malicioso.

Sin embargo, el script activador que proporciona <a href="https://msguides.com/" target="_blank">msguides</a> parece bastante transparente. El código con comentarios del activador KMS se encuentra en la sección **Anexo Script con comentarios.**

# Anexo Script KMS con comentarios

~~~bat
:: ========================================================
:: ========================================================
:: ========================================================

:: Aquí separamos el script de una manera únicamente ilustrativa, si se trata de correr nos dará un error porque se pierde el formato.
:: No necesariamente se ha interpretado correctamente el script, pero se ha hecho el mejor esfuerzo

:: ========================================================
:: ========================================================
:: ========================================================

:: @echo off  => define que la terminal no nos devuelva las salidas propias de la terminal
:: Es muy usado en los scripts para evitar la salida de información basura
@echo off

:: title => Crea un título para la ventana
title _ Permanently Activate Office 365 ProPlus for FREE => MSGuides . com

:: & => Es utilizado para concatenar comandos. Usado cuando escribes en una solo línea. 
:: cls => Hace un limpiado de la pantalla para que no muestre lo escritor anteriormente
cls

:: Pone los mensajes que nosotros vemos
:: echo. hace un salto de línea
echo ============================================================================
echo #Project: Activating Microsoft software products for FREE without software
echo ============================================================================
echo.
echo #Supported products: Office 365 ProPlus (x86-x64)
echo.
echo.

:: & => Es utilizado para concatenar comandos. Usado cuando escribes en una solo línea. 
:: ( ) => Es usado para agrupar una secuencia de comandos
&
(
    :: Condicion if que checa la existencia de un archivo .vbs (Visual Basic Script)
    :: En caso de que exista entramos a la carpeta 
    :: Esto se hace para Office 2016 versión de x64 bits
    if exist "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" 
        cd /d "%ProgramFiles%\Microsoft Office\Office16"
)
&(  
    :: Condicion if que checa la existencia de un archivo .vbs (Visual Basic Script)
    :: En caso de que exista entramos a la carpeta 
    :: Esto se hace para Office 2016 versión de x32 bits
    if exist "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" 
        cd /d "%ProgramFiles(x86)%\Microsoft Office\Office16"
)
&(
    :: Para cada uno de los archivos que cumplan con una expresión regular ejecuta un script 
        :: /inslic Instala una licencia con la ruta proporcionada de la licencia .xrm-ms. Se requiere un parámetro de valor
        :: Esto lo realiza para preparar las llaves KMS
        :: Corroborar el output del Anexo A
    for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_kms*.xrm-ms') 
        do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)
&(
    :: Para cada uno de los archivos que cumplan con una expresión regular ejecuta un script 
        :: /inslic Instala una licencia con la ruta proporcionada de la licencia .xrm-ms Se requiere un parámetro de valor
        :: Esto lo realiza para preparar las llaves MAK
        :: Corroborar el output del Anexo A
    for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_mak*.xrm-ms') 
        do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" >nul
)

:: Mensajes que nosotros vemos echo. es un salto de linea
echo.
echo ============================================================================
echo Activating your Office...

:: Esta secuenta de comandos quita la licencia de prueba actual de office. Dependiendo de nuestra versión acertará en alguna

&cscript //nologo ospp.vbs /unpkey:WFG99 >nul
&cscript //nologo ospp.vbs /unpkey:DRTFM >nul
&cscript //nologo ospp.vbs /unpkey:BTDRB >nul

:: Registra la nueva licencia de office y procede a realizar una activación por medio de KMS.
    :: Probará al menos esos tres servidores kms7, kms8, kms9
&cscript //nologo ospp.vbs /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99 >nul
&set i=1:server
    if %i%==1 set KMS_Sev=kms7.MSGuides.com
    if %i%==2 set KMS_Sev=kms8.MSGuides.com
    if %i%==3 set KMS_Sev=kms9.MSGuides.com
    if %i%==4 goto notsupported
    cscript //nologo ospp.vbs /sethst:%KMS_Sev% >nul

:: Mensajes que nosotros vemos echo. es un salto de linea
echo ============================================================================
echo.
echo.

:: Realiza la activación

cscript //nologo ospp.vbs /act | find /i "successful" 


:: Mensajes que nosotros vemos echo. es un salto de linea
& (echo.
echo ============================================================================
echo.
echo #My official blog: MSGuides.com
echo.
echo #How it works: bit.ly/kms-server
echo.
echo #Please feel free to contact me at msguides.com@gmail.com if you have any questions or concerns.
echo.
echo #Please consider supporting this project: donate.msguides.com
echo #Your support is helping me keep my servers running everyday!
echo.
echo ============================================================================

:: Realiza una pregunta para saber si quieres mirar su blog

choice /n /c YN /m "Would you like to visit my blog [Y,N]?" 
& if errorlevel 2 exit) || (echo The connection to my KMS server failed! Trying to connect to another one... 
:: Mensajes que nosotros vemos echo. es un salto de linea
& echo Please wait... 
& echo. 
& echo. 

& set /a i+=1 
& goto server)
explorer "http://MSGuides.com"
&goto halt

:notsupported

:: Mensajes que nosotros vemos echo. es un salto de linea
echo.
echo ============================================================================
echo Sorry! Your version is not supported.
echo Please try installing the latest version here: bit.ly/odt2k16

:halt
pause >nul


:: Anexo A
    :: Se puede visualizar la quitar todos los ">nul" del script
:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_kms_client-ppd.xrm-ms
:: Office license installed successfully.
:: ---------------------------------------
:: ---Exiting-----------------------------
:: Microsoft (R) Windows Script Host Version 5.812
:: Copyright (C) Microsoft Corporation. All rights reserved.
:: 
:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_kms_client-ul-oob.xrm-ms
:: Office license installed successfully.
:: ---------------------------------------
:: ---Exiting-----------------------------
:: Microsoft (R) Windows Script Host Version 5.812
:: Copyright (C) Microsoft Corporation. All rights reserved.
:: 
:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_kms_client-ul.xrm-ms
:: Office license installed successfully.
:: ---------------------------------------
:: ---Exiting-----------------------------
:: Microsoft (R) Windows Script Host Version 5.812
:: Copyright (C) Microsoft Corporation. All rights reserved.
:: 
:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_mak-pl.xrm-ms
:: Office license installed successfully.
:: ---------------------------------------
:: ---Exiting-----------------------------
:: Microsoft (R) Windows Script Host Version 5.812
:: Copyright (C) Microsoft Corporation. All rights reserved.
:: 
:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_mak-ppd.xrm-ms
:: Office license installed successfully.
:: ---------------------------------------
:: ---Exiting-----------------------------
:: Microsoft (R) Windows Script Host Version 5.812
:: Copyright (C) Microsoft Corporation. All rights reserved.

:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_mak-ul-oob.xrm-ms
:: Office license installed successfully.
:: ---------------------------------------
:: ---Exiting-----------------------------
:: Microsoft (R) Windows Script Host Version 5.812
:: Copyright (C) Microsoft Corporation. All rights reserved.
:: 
:: ---Processing--------------------------
:: ---------------------------------------
:: Installing Office license: ..\root\licenses16\proplusvl_mak-ul-phn.xrm-ms
:: Office license installed successfully.
---------------------------------------
:: ---Exiting-----------------------------
:: 
:: ============================================================================
~~~

+ Recomendamos mejor visualizar el código desde el repositorio de Github para ver los colores referentes al código.
+ :: Son comentarios

# Problemas / Soluciones

## Windows detecta activate como virus: 

Lo que sucede es que windows detecta el script como un activador de Office y por supuesto lo bloquea. De hecho, en la descripción de Windows Defender menciona. *“HackTool:BAT/AutoKMS This program has potentially unwanted behavior”*

+ Basta con deshabilitar Windows Defender durante la activación. 
    + <a href="https://elcaza.github.io/blog/posts/windows/on_off_windows_defender/" target="_blank">Aquí detallamos el proceso</a>

## Pantalla azul de bloqueo

Pantalla azul de bloqueo:
Al igual que Windows Defender — Real Time Protection es necesario desactivar App & Browser Control — Check apps and files

## ¡No sé la contraseña del zip!

Bro, ese no es un problema. La contraseña es `activate` como se mencionó en el post.

# Vídeo sobre este artículo

*Pendiente. Quizá exista, quizá no.

:::tip[Nota final]
Gracias por terminar de leer este artículo, recuerda tener cuidado con lo que le instalas a tu compu :)

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>