---
title: iOS Jailbreak
published: 2026-02-22
description: 'Notas sobre pruebas de seguridad a iOS'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/ios_jailbreak/portada.jpg'
tags: [Hacking, iOS]
category: 'Hacking'
draft: false 
lang: 'es'
---

# Teoría

## ¿Por qué la gente hace Jailbreak?
Las personas pueden optar por realizar el jailbreak de sus dispositivos para realizar acciones que no estén permitidas por Apple. Dos casos de uso pueden ser:
1. Realizar pruebas de seguridad informática
1. Agregar "Tweaks" (ajustes, personalizaciones) a su iPhone

## Tipos de Jailbreak
+ rootful
	+ Da acceso completo al sistema, también al sistema raíz "/". Te da privilegios de root
+ rootless
	+ No tienes acceso completo al sistema, no puedes acceder a raíz "/", pero sí a otros directorios. También tienes privilegios de root.

## Tipos de persistencia
+ Tethered
	+ 📱 🔄 📵 💻 ✅
	+ Requiere la conexión a una computadora cada vez que se reinica. Sin esto el teléfono no iniciará de manera correcta.
+ Untethered
	+ 📱 🔄 ✅
	+ El teléfono puede apagarse y reiniciarse sin perder funcionalidades ni su estado de jailbreak. Sin embargo, este tipo de jailbreak comenzó a desaparecer con iOS 9
+ Semi-tethered
	+ 📱 🔄 📱🔒 💻 ✅
	+ Si el teléfono se apaga o reinicia, el teléfono podrá seguir utilizandose pero perderá su estado de jailbreak. Este puede volver a activarse mediante el mismo proceso que le hizo jailbreak en un inicio.
	+ Actualmente, este es el jailbreak más utilizado.
+ Semi-Untethered Jailbreak
	+ 📱 🔄 📱🔒 📲 ✅
	+ Si el teléfono se apaga o reinicia, el teléfono podrá seguir utilizandose pero perderá su estado de jailbreak. A diferencia del "semi-tethered" este puede volver a activarse mediante una aplicación que devuelva el estado de jailbreak.


************************************************************************************************

# Requisitos para realizar jailbreak 

1. Dispositivo soportado
    - Puedes ver la lista actualizada en <a href="https://theapplewiki.com/wiki/Jailbreak" target="_blank">theapplewiki</a>  
1. Botones funcionales
	- Lo requieres para entrar al modo DFU
1. iPhone sin bloqueos ni cuentas
	- Esto no tendría que justificarlo, ¿o sí?
1. Tener espacio disponible en el celuar (10GB)
	- Para rootful con FakeFS
1. Restaura el celular y no actives las opciones de seguridad:
	- FaceID, Passcode, TouchID, desactivar Find Iphone
	- Entonces, lo restauras a fabrica y le pones "No usar/Configurar más tarde"
	- En caso de que uses Ipad 7th debes habilitar el "Developer Mode"
		- `Settings => Privacy & Security => Developer Mode (Off => ON)`

## Resumen de dispositivos recomendados
- iPhone 8 y X
- Ipad de 7th generación (Esta es la que tendrá el iOS más reciente)
- Se podría un iPhone XR, iPhone XS, iPhone XS Max, pero es poco probable que lo encuentres con iOS 17.0

## Notas generales
+ Es un proceso que puede tomar un par de intentos, persevera.
+ Debes conocer cómo entrar al modo DFU
	+ Como los trucos en GTA
+ A través de un cables **USB A** es más fácil que se realice el proceso correctamente. USB C, puede fallar
+ Es más fácil a través de los puertos **USB 2.0** de la computadora
+ A través de la ISO es más sencillo que el proceso tenga éxito

************************************************************************************************

# ¿Cómo entrar al modo DFU?
+ Iphone 8 - Palera1n (live usb)
	+ ⬆️ ⬇️ (⏻)_hasta_apagar (⬇️⏻)_5s (⬇️)_hasta_comenzar_el_proceso
	+ UP, DOWN, SIDE-hasta-apagar, SOLTAR_TODO, SIDE+DOWN-5segundos, DOWN-5segundos (o hasta que se comience el proceso)
	+ PRESION_RAPIDA, PRESION_RAPIDA, SOSTENER, SOLTAR_TODO, PRESIONAR_AMBOS_SOSTENER, SOLTAR_SOLO_POWER

+ IPAD 7 e Iphone 7 - Palera1n (live usb)
	+ (T)(⏻)_hasta_apagar (T⏻)_5s (T)_hasta_comenzar_el_proceso
	+ (TOUCH, POWER)_hasta-apagar, SOLTAR_TODO, (TOUCH, POWER)_5segundos, TOUCH-5segundos (o hasta que se comience el proceso)
	+ SOSTENER, SOLTAR_TODO, PRESIONAR_AMBOS_SOSTENER, SOLTAR_SOLO_POWER

************************************************************************************************

# Palera1n
## Notas 
+ Requieres 10GB libres de almacenamiento
+ Dale a `Trust` cuando lo conectes a la computadora

## Proceso

### 1. Previo
+ Crear USB Booteable de <a href="https://palera.in/" target="_blank">palera1n</a>
+ Iniciar con el ISO
+ Seleccionar Palera1n

### 2. Creación del Fake FS (solo la primera vez)
+ Si es primera vez configurar las siguientes opciones
	+ Selecciona rootful o rootless
	+ 1 Create FakeFS
	+ 3 Verbose
	+ 8 Debug
	+ Arguments: -f -c -V -v
+ Conectar tu celular y ponerlo en modo DFU
+ Dejar que trabaje y, una vez finalizado podrás notar que tienes 10Gb menos en tu almacenamiento

### 3. Inicializar jailbreak (Cada vez que se reinicia/apaga el dispositivo)
+ Iniciar con el ISO
+ Seleccionar Palera1n
+ Seleccionar las siguientes opciones
	+ Selecciona rootful o rootless
	+ 3 Verbose
	+ 8 Debug
	+ Arguments: -f -V -v
+ Conectar tu celular y ponerlo en modo DFU
+ Dejar que trabaje y, una vez finalizado, podrás ver que tienes instalado palera1n y podrás instalar Sileo o Zebra.

### 4. Una vez hecho el jailbreak
+ <a href="https://elcaza.github.io/posts/hacking/ios_herramientas_pentest/" target="_blank">Herramientas para realizar pruebas de seguridad en iOS</a>

## Problemas y soluciones
+ Reiniciar el programa
	+ `CTRL +C`
	+ `exit`
+ Se me apagó el celular y ahora no tengo mi celular con Jailbreak
	+ Solamente haz el paso 3 para volver a inicializar el jailbreak

# Otros recursos
+ <a href="https://theapplewiki.com/wiki/Jailbreak" target="_blank">theapplewiki Jailbreak</a>
+ <a href="https://ios.cfw.guide/installing-trollstore/" target="_blank">trollstore</a>

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