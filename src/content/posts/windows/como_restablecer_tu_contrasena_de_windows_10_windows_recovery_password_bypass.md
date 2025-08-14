---
title: ¿Cómo restablecer tu contraseña de Windows 10? — Windows recovery password bypass
published: 2020-10-25
description: '¿Cómo restaurar el password de windows de manera forzada mediante la ejecución de una consola en modo administrador sin conocer ningun password?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/1.webp'
tags: [Windows]
category: 'Windows'
draft: false 
lang: 'es'
---

Cuando olvidamos nuestra contraseña de Windows, Microsoft nos recomienda algunas opciones para recuperarla:

+ Recuperar tu password si tu cuenta está vinculada con Microsoft
+ Recuperar tu password por medio de una pregunta secreta
+ Recuperar tu password mediante un archivo de recuperación

Sin embargo, estas opciones a menudo no serán de nuestra utilidad. No obstante, existe una manera en que podemos **restaurar el password de windows de manera forzada** mediante la ejecución de una **consola en modo administrador sin conocer ningun password**.

# Contenido

1. Explicación del problema y solución
1. Iniciando una consola
1. Sustituir `utilman.exe` por `cmd.exe`
1. Cambiando la contraseña
1. Regresando las `utilman.exe` a la normalidad
1. Anexo: Preparación de vmware (Solo si tu máquina es virtual)

# Requisitos:

+ Tener un CD/USB booteable con Windows 10 o algún Linux con Live mode

# 1. Explicación del problema y solución

Cuando perdemos nuestra contraseña no nos será posible acceder a nuestra cuenta o utilizar CMD para cambiar nuestro password desde “Recuperación de sistema” que está integrado en Windows 10 (Antes sí era posible).

Sin embargo, es posible ejecutar el cambio de contraseña de un usuario haciendo una modificación en la pantalla de `Login` de Windows. Esto lo logramos sustituyendo `utilman.exe`, una utilería que sirve para mostrar la opciones de accesibilidad del sistema

Icono que activa utilman.exe

<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/2.webp">

Acción comun de utilman.exe

<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/3.webp">

Ahora, nosotros cambiaremos el binario utilman.exe por el binario de cmd.exe, con lo que al dar click sobre el icono de utilman se abrirá una **consola cmd privilegiada**. Entonces, nuestro ataque consistirá en:

1. Bootear desde una USB/CD externo<Por  En sus > que permita modificar los archivos del sistema operativo.
1. Sustituir el binario de **utilman.exe** por el binario **cmd.exe**
1. Reiniciar y ejecutar nuestro cmd para cambiar el password de nuestra cuenta
1. Restaurar el archivo *utilman.exe*

# 2. Iniciando una consola

Para iniciar una consola que pueda realizar cambios sobre el sistema de archivos de nuestro Sistema Operativo es necesario bootear desde un medio extraible. Generalmente un CD ó USB Booteable. Dos formas populares son las siguientes

+ Utilizar una USB/CD de Windows 10
+ Utilizar una USB/CD de algún Linux con live mode

Para este ejemplo usaremos un USB Booteable con Windows y los pasos serán los siguientes:

1. Se inserta la USB/CD booteable
1. Iniciamos en modo solución de problemas
1. Booteamos de desde nuestro USB/CD booteable
1. Ejecutamos una CMD

Una vez insertado nuestro USB/CD booteable. Desde la página de login de Windows, damos click en “Botón de apagado”, presionando la tecla **SHIFT** damos click en “Restart”.

(También podríamos simplemente bootear desde el inicio de Windows, el proceso variará de acuerdo a la marca de nuestra computadora).

Iniciamos en modo solución de errores
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/4.webp">

Tecla Shift en el teclado (De nada, tía).
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/5.webp">


Esto nos abrirá las opciones de solución de problemas de Windows 10 y ejecutaremos los siguientes pasos:

Seleccionamos usar un dispositivo
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/6.webp">

Seleccionamos nuestro CD/USB, según sea el caso.
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/7.webp">

Una vez que veamos esta pantalla presionamos cualquier tecla para iniciar desde el CD/DVD
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/8.webp">

Entonces iniciará nuestro CD/USB.
Configuramos nuestro idioma y damos siguiente
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/9.webp">

Y seleccionamos reparar computadora
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/10.webp">

Entonces nuevamente llegaremos a opciones de reparación y elegiremos lo siguiente:

Reparación
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/11.webp">

Usar la línea de comandos
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/12.webp">

Y es así como obtendremos nuestra consola **CMD** en modo administrador.
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/13.webp">

# 3. Sustituir utilman.exe por cmd.exe

Ahora que tenemos un interprete de comandos debemos

1. Ubicar dónde está nuestra instalación de Windows
1. Renombrar utilman.exe por *utilman.exe.bk*
1. Crear una copia de *cmd.exe* y llamarla utilman.exe
1. Reiniciar nuestro sistema operativo

Entonces, para ubicar en lugar en que se encuentra nuestro windows usamos

~~~bat
fsutil fsinfo drives
~~~

Esto nos mostrará que unidades tenemos montadas:
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/14.webp">

+ X:\ Es nuestro unidad actual, por lo que ahí no puede estar nuestra instalación de Windows.
+ C:\ y D:\ podrían alojar nuestra instalación

Entonces seleccionamos “unidad:” + Enter para acceder a ella y “dir” para ver qué hay dentro de ella. Por ejemplo, para la unidad “C:” haremos lo siguiente:

~~~bat
c:
dir
~~~

De esa manera encontramos nuestra unidad que tiene Windows instalado
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/15.webp">

+ Puede que en su caso sea **“d:”** en lugar de **“c:”**, lo que le mostrará que está en el sitio correcto son las carpetas que encuentre dentro. (El recuadro amarillo a su derecha)

Ahora:

1. Abrimos la ubicación de utilman.exe que es `C:\Windows\System32`
1. Renombramos el archivo `Utilman.exe` por `Utilman.exe.bak`
1. Copiamos cmd.exe y lo nombramos como `Utilman.exe` (Con esto haremos que cada que abramos utilman desde la pantalla de login se ejecute la línea de comandos en modo administrador).
1. Reiniciamos nuestro equipo

~~~bat
cd Windows\System32

ren Utilman.exe Utilman.exe.bak
copy cmd.exe Utilman.exe

shutdown -r -t 0
~~~

Paso 1
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/16.webp">

Paso 2 a 4
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/17.webp">

# 4. Cambiando la contraseña

Una vez hecho lo anterior regresaremos a nuestra pantalla normal de login, sin embargo ahora podremos ejecutar el cambio de contraseña desde el botón de *utilman.exe*


Damos click al botón de asistencia (el que ejecuta utilman.exe)
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/18.webp">

Y obtenemos nuestro CMD en modo administrador
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/19.webp">

Ahora simplemente procederemos a:
1. Ubicar el nombre de la cuenta a la que vamos a reiniciarle el password
1. Cambiar el password
1. Comprobar que ha funcionado

Para listar los usuarios del sistema ejecutamos:

~~~bat
net user
~~~

Usuarios registrados en Windows
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/20.webp">

En este caso el usuario al que le restauraremos la contraseña es user, y podemos hacerlo de dos maneras:

## a) Usar el modo interactivo donde llenaremos el password desde la línea de comandos

~~~bat
net user user *
~~~

Modo interactivo
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/21.webp">

## b) Usar el modo directo, usuario y password a establecer.

~~~bat
net user user your-new-password
~~~

Modo directo
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/22.webp">

+ Hay que notar que el segundo “user” en net user es el nombre de usuario. Por lo que quedaría `net user tu_usuario opcion`

Y ahora podemos iniciar sesión.

Inicio exitoso con la nueva contraseña
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/23.webp">

# 5. Regresando las utilman.exe a la normalidad

:::warning[Importante]
Finalmente debemos regresar la funcionalidad normal de utilman.exe, esto debido a que no queremos que otro usuario pueda hacer uso de nuestro cmd con privilegios. ¡Es riesgoso!
:::

Para esto haremos lo siguiente:
1. Repetiremos los pasos del “punto 2” <<“2) Iniciando una consola” >> para obtener nuestra consola
1. Restableceremos los archivos como estaban en un inicio
1. Reiniciaremos nuestro equipo

Consola de administrador
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/24.webp">

Listado del sistema de archivos y entrada al directorio
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/25.webp">

Una vez que obtenemos nuestra consola y nos posicionamos en C:\Windows\System32

1. Removemos nuestro falso Utilman.exe
1. Renombramos nuestro Utilman.exe.bak por Utilman.exe
1. Reiniciamos nuestro equipo

~~~bat
del Utilman.exe
ren Utilman.exe.bak Utilman.exe
shutdown -r -t 0
~~~

<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/26.webp">

Y podemos comprobar que Utilman ahora vuelve a tener sus funcionalidades normales.

Utilman con sus funciones normales
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/27.webp">

# 6. Anexo: Preparación de vmware (Solo si tu máquina es virtual)

En caso de que estés corriendo Windows 10 sobre una máquina virtual de vmware la configuración es la siguiente:

Settings de la máquina
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/28.webp">

Selección de nuestra imagen ISO de Windows 10 y opción “Connect atpower on”
<img witdh=100% src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/recovery_pass_w10/29.webp">

:::tip[Nota final]
¡Gracias por terminar de leer este artículo!

— El Capitán
:::

+ <a href="https://twitter.com/elcaza_" target="_blank">Twitter</a>
+ <a href="https://github.com/elcaza" target="_blank">Github</a>