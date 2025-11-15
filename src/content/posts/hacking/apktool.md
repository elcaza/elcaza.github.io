---
title: Decompilar y compilar aplicaciones móviles con apktool
published: 2025-11-13
description: 'Decompilar y compilar aplicaciones móviles con apktool'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/apktool/portada.jpg'
tags: [Hacking, Android]
category: 'Hacking'
draft: false 
lang: 'es'
---

# ¿Qué es apktool?
Apktool es una herramienta para la ingeniería inversa de aplicaciones de Android. Apktool permite descompilar los archivos APK para obtener los recursos y el código, hacer modificaciones y luego recompilarlos de nuevo en un APK.

## Procedimiento
1. Decompilar la aplicación
1. Aplicar modificaciones
1. Construir la aplicación modificada
1. Alineación mediante zipalign
1. Firma de la aplicación con apksigner
1. Instalar la aplicación

# ¿Cómo instalar?
Para realizar la instalación solamente debes seguir los siguientes pasos:
+ <a href="https://apktool.org/" target="_blank">Sitio web oficial</a>

# Guía paso a paso
## 1-3 Decompilar la aplicación, hacer modificaciones y construir la aplicación
+ The code smali can be modificated
http://pallergabor.uw.hu/androidblog/dalvik_opcodes.html

~~~bash
# Decompilar la aplicación
apktool d your_app.apk

# Haces las modificaciones

# Compilar la aplicación modificada
apktool b directory -o patched_app.apk
~~~

## 4 Alineación mediante zipalign
Zipalign es una herramienta de optimización de archivos de archivo proporcionada por Android SDK
+ Función principal: Asegurar que todos los datos sin comprimir (como las imágenes y los archivos de recursos) dentro del APK estén alineados en límites de 4 bytes relativos al inicio del archivo.
+ Propósito: Esta alineación permite que el sistema operativo Android acceda directamente a los archivos del APK a través del mapeo de memoria (mmap) sin tener que descomprimir los datos primero.
+ Beneficio: Resulta en un menor uso de la RAM de la aplicación, una ejecución más rápida y un menor consumo de batería. Debe ejecutarse antes de la firma.

### ¿Cómo encontrarlo?
Debes tener Android Studio instalado
~~~bash
find / -name "zipalign" 2> /dev/null
~~~

### ¿Cómo usarlo?
~~~bash
/home/elcaza/Android/Sdk/build-tools/36.0.0/zipalign -v 4 patched_app.apk patched_app_aligned.apk
~~~
+ Cambia `elcaza` por tu `usuario`
+ La ruta es la que salio en la sección de "¿Cómo encontrarlo?"

## 5 Firma de la aplicación con apksigner
Apksigner es una herramienta de firma de APK, introducida a partir de la versión 24.0.3 del Android SDK Build Tools. Reemplaza a la herramienta jarsigner para fines de firma de APK.

Requieres un keystore. Puedes usar uno que ya tengas o generar uno simple para pruebas con keytool.

### Creación de certificado con keytool
~~~bash
keytool -genkey -v -keystore debug.keystore -storepass android -alias androiddebugkey -keypass android -keyalg RSA -keysize 2048 -validity 10000
~~~
+ Se establece storepass con el valor android 
+ Se crea el archivo debug.keystore

### Firma de la aplicación

#### ¿Cómo encontrarlo?
Debes tener Android Studio instalado
~~~bash
find / -name "apksigner" 2> /dev/null
~~~

#### ¿Cómo usarlo?
~~~bash
/home/elcaza/Android/Sdk/build-tools/36.0.0/apksigner sign --ks debug.keystore --ks-key-alias androiddebugkey patched_app_aligned.apk
~~~
+ La contraseña establecida en el paso anterior fue `android`

## 6 Instalar la aplicación
~~~bash
adb install patched_app_aligned.apk
~~~

# Links
## Documentación oficial
+ <a href="https://apktool.org/" target="_blank">Sitio web oficial</a>
+ <a href="https://github.com/iBotPeaches/Apktool" target="_blank">Repositorio de Github</a>

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