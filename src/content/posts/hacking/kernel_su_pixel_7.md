---
title: Kernel SU en Pixel 7 Pro
published: 2026-03-14
description: Rootear un Pixel 7 Pro con KernelSU
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/kernel_su_pixel_7/portada.jpg'
tags: [Hacking, Mobile, Android]
category: Hacking
draft: false
lang: es
---

:::caution
Querido Antonio del futuro, toma este post con cautela porque esto solamente lo has probado una vez. Al regresar por tus recuerdos podrías haber omitido detalles menores o podrían haber cambiado las cosas.
:::

## Requisitos Previos

1. **Bootloader Desbloqueado:** Este proceso borrará todos tus datos si aún no lo has desbloqueado.
2. Developer options activadas
3. Depuración USB (USB debugging) habilitada
4. **Drivers y Platform Tools:** Tener instalado `adb` y `fastboot` en tu PC (preferentemente en Debian o Windows).
5. **Imagen de Fábrica:** Descarga la imagen exacta que coincida con tu número de compilación actual desde el [sitio oficial de Google](https://developers.google.com/android/images).


### Habilitar developer options

- Settings - About phone - Build number (press 7 times) => You are now developer
### Desbloquear bootloader en configuraciones

- System - Developer options - OEM unlocking (Desbloqueo de OEM)
- Activar USB debugging 
- Nota: Los modelos bloqueados por operadora (por ejemplo, Verizon) no permiten activar "OEM unlocking".
### Desbloquear bootloader con fastboot

Esto borrará todos tus archivos. Lo mejor es que lo hagas sin una cuenta de Google vinculada.

~~~bash
# Reinicia el teléfono en modo fastboot
adb reboot bootloader

fastboot flashing unlock
~~~

- Selecciona *Unlock the bootloader* con los botones de volumen y confirma con el botón de encendido

## Paso 1: Identificar la versión del Kernel

La app de KernelSU indica si tu dispositivo es compatible.

- Ve a **Ajustes > Información del teléfono > Versión de Android**.
- Busca la "Versión del kernel". También puedes verla con `adb shell uname -r`.
- Verifica el Build number. En este caso `bp4a.251205.006`

## Paso 2: Obtener el archivo init_boot

1. Descomprime el archivo `.zip` de la imagen de fábrica que descargaste.
2. Dentro encontrarás otro `.zip` (image-cheetah-xxxx.zip); ábrelo.
3. Extrae el archivo **`init_boot.img`**. (Nota: los dispositivos que salieron con Android 13 o posterior, como el Pixel 7 Pro, tienen una partición `init_boot`; en otros se parchea `boot.img`).
4. Guarda una copia del `init_boot.img` original: la necesitarás para revertir los cambios.

## Paso 3: Instalar la App de KernelSU y Parchear

1. Descarga e instala el APK de **KernelSU Next Manager** desde su repositorio oficial en GitHub.
	1. https://github.com/KernelSU-Next/KernelSU-Next
	2. KernelSU_Next_v3.1.0_33024-release.apk => Versión normal del manager
	3. KernelSU_Next_v3.1.0-spoofed_33024-release.apk => Variante "spoofed" del manager (consulta en el repositorio qué modifica)
2. Copia el archivo `init_boot.img` de tu PC al almacenamiento interno del Pixel.
	1. `adb push init_boot.img /sdcard/Download/`
3. Abre la app KernelSU
	1. Click to install
	2. Select a file
	3. Selecciona tu imagen
	4. Next
	5. En el output se verá la ruta en que guardó tu imagen
4. Busca tu `init_boot.img`. La app generará un archivo nuevo llamado algo como `kernelsu_patched_xxx.img`.
5. Copia ese archivo de vuelta a tu PC.
	1. Por ejemplo: `adb pull /storage/emulated/0/Download/kernelsu_next_patched_20260314_165450.img`


## Paso 4: Flashear mediante Fastboot

1. Apaga el teléfono 
2. entra en **Modo Fastboot** (Mantén Presionado Power + Bajar Volumen).


Otra opción:

~~~bash
adb reboot bootloader
~~~
 
Conéctalo a la PC y verifica la conexión:

~~~bash
fastboot devices
~~~
    
Flashea la imagen parcheada en la partición correspondiente:

~~~bash
fastboot flash init_boot kernelsu_next_patched_20260314_165450.img 
~~~

Reinicia el dispositivo:

~~~bash
fastboot reboot
~~~


---

## Paso 5: Verificación

Abre la aplicación **KernelSU Manager**. Si todo salió bien, verás el estado como **"Working"** o **Installed**. Desde aquí podrás gestionar los permisos de superusuario para tus aplicaciones y módulos.

## Revertir los cambios y actualizaciones

- **Si el teléfono no arranca**, entra en modo fastboot y flashea el `init_boot.img` original:

~~~bash
fastboot flash init_boot init_boot.img
~~~

- **Después de una actualización del sistema** se pierde el root. Vuelve a parchear el `init_boot.img` de la nueva versión o, si la app lo ofrece, usa "Install to inactive slot".
- **No vuelvas a bloquear el bootloader con una imagen modificada:** el teléfono puede quedar inservible. Primero restaura la imagen original.

---

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
