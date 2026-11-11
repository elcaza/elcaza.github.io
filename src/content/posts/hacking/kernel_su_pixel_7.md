---
title: Kernel SU en Pixel 7 Pro
published: 2026-03-14
description: Notas sobre pruebas de seguridad a Android
image: https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/hacking_android/portada.jpg
tags: [Hacking, Android, iOS]
category: Hacking
draft: true
lang: es
---

# Kernel SU en Pixel 7 Pro
## Requisitos Previos

1. **Bootloader Desbloqueado:** Este proceso borrará todos tus datos si aún no lo has desbloqueado.
2. Developer options activadas
3. USB Debbugin habilitado
4. **Drivers y Platform Tools:** Tener instalado `adb` y `fastboot` en tu PC (preferentemente en Debian o Windows).
5. **Imagen de Fábrica:** Descarga la imagen exacta que coincida con tu número de compilación actual desde el [sitio oficial de Google](https://developers.google.com/android/images).


### Habilitar developer options

- Settings - About phone - Build number (press 7 times) => You are now developer
### Desbloquear bootloader en configuraciones

- System - Developer options - OEM Unlocked
- Activar USB debugging 
### Desbloquear bootloader con fastboot

Esto borrará todos tus archivos. Lo mejor es que lo hagas sin una cuenta de Google vinculada.

~~~bash
fastboot flashing unlock
~~~

- Aceptar con las teclas de bloqueo y subir y bajar volumen

## Paso 1: Identificar la versión del Kernel

KernelSU requiere una versión de kernel específica ($GKI$ - Generic Kernel Image).

- Ve a **Ajustes > Información del teléfono > Versión de Android**.
- Busca la "Versión del kernel". Debería ser algo como `6.1.xxx`.
- Verifica el Build number. En este caso `bp4a.251205.006`

## Paso 2: Obtener el archivo init_boot

1. Descomprime el archivo `.zip` de la imagen de fábrica que descargaste.
2. Dentro encontrarás otro `.zip` (image-cheetah-xxxx.zip); ábrelo.
3. Extrae el archivo **`init_boot.img`**. (Nota: En dispositivos con Kernel 5.10+ , se usa `init_boot` en lugar de `boot`).

## Paso 3: Instalar la App de KernelSU y Parchear

1. Descarga e instala el APK de **KernelSU Manager** desde su repositorio oficial en GitHub.
	1. https://github.com/tiann/KernelSU
	2. KernelSU_Next_v3.1.0_33024-release.apk => Normal, sin ocultar firmas
	3. KernelSU_Next_v3.1.0-spoofed_33024-release.apk => Oculta su presencia. Bypass de seguridad de apps móviles o apps bancarias 
2. Copia el archivo `init_boot.img` de tu PC al almacenamiento interno del Pixel.
	1. Puedes pasar los archivos con `python3 -m http.server` y luego visitando el sitio web
3. Abre la app KernelSU
	1. Click to install
	2. Select a file
	3. Selecciona tu imagen
	4. Next
	5. En el output se verá la futa en que guardó tu imagen
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

```bash
fastboot devices
```
    
Flashea la imagen parcheada en la partición correspondiente:

```bash
fastboot flash init_boot kernelsu_next_patched_20260314_165450.img 
```

Reinicia el dispositivo:

```bash
fastboot reboot
```


---

## Paso 5: Verificación

Abre la aplicación **KernelSU Manager**. Si todo salió bien, verás el estado como **"Working"** o **Installed**. Desde aquí podrás gestionar los permisos de superusuario para tus aplicaciones y módulos.

## Recomendación de Seguridad

Dado que trabajas con herramientas de seguridad y entornos de red, recuerda que KernelSU es más "invisible" que Magisk ante mecanismos como Play Integrity, pero aun así es recomendable utilizar módulos como _ZygiskNext_ si necesitas ocultar el estado del bootloader ante apps bancarias o corporativas.

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