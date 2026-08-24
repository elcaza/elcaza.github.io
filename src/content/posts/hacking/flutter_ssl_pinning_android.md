---
title: Deshabilitar la verificación TLS / SSL Pinning de Flutter (Android)
published: 2026-08-23
description: 'Deshabilitar el SSL Pinning de Flutter y Dart en Android'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/ios_android_flutter_ssl_pinning/portada.jpeg'
tags: [Hacking, Android]
category: 'Hacking'
draft: false 
lang: 'es'
---

# Deshabilitar las protecciones TLS en aplicativos hechos con Flutter

Las aplicaciones desarrolladas con Flutter/Dart evaden los métodos tradicionales de intercepción de tráfico. Al ignorar el proxy configurado en los ajustes de Android y descartar las CAs instaladas en el almacenamiento del sistema operativo (User/System), la interceptación mediante Burp Suite falla debido a que las peticiones nunca llegan al proxy. Este documento detalla el procedimiento para forzar el enrutamiento a nivel de Kernel (`iptables`) y neutralizar la validación TLS en memoria mediante instrumentación dinámica (`Frida`).

# Just do it - Breve resumen de la solución

## 0. Requisitos:
+ Proxy en modo invisible
+ Celular Android con configuración de reglas iptables
  + Pixel 7 Pro - Android 16 - Frida 17.17.0
+ Script `disable-flutter-tls-verification` de `NVISOsecurity`
  + <a href="https://github.com/NVISOsecurity/disable-flutter-tls-verification" target="_blank">disable-flutter-tls-verification</a>

## 1. Proxy en modo invisible
Configuras Burp en modo "Invisible proxy" y escuchando en una interfaz
1. Proxy => HTTP Proxy => Proxy Settings => Proxy listeners => Interface Edit => Request handling => **Support invisible proxying** 
   1. La interfaz NO debe ser localhost

## 2. Configuración de iptables en Android
Envío de las peticiones locales desde Android hacia Burp Suite mediante iptables

### Ver reglas de iptables actuales dentro de android

~~~bash
adb shell "su -c 'iptables -t nat -L -n -v --line-numbers'"
~~~

### Celular <=> Proxy

Variables a configurar
~~~bash
PROXY_IP=10.1.1.5
PROXY_PORT=8888
~~~

Para crear las reglas
~~~bash
# Redirigir tráfico HTTP y HTTPS de todo el sistema hacia Burp
adb shell "su -c 'iptables -t nat -A OUTPUT -p tcp --dport 80 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT'"
adb shell "su -c 'iptables -t nat -A OUTPUT -p tcp --dport 443 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT'"

# Habilitar MASQUERADE para asegurar la vuelta del tráfico
adb shell "su -c 'iptables -t nat -A POSTROUTING -p tcp --dport $PROXY_PORT -j MASQUERADE'"
~~~

Para remover las reglas
~~~bash
adb shell "su -c 'iptables -t nat -D OUTPUT -p tcp --dport 80 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT'"
adb shell "su -c 'iptables -t nat -D OUTPUT -p tcp --dport 443 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT'"
adb shell "su -c 'iptables -t nat -D POSTROUTING -p tcp --dport $PROXY_PORT -j MASQUERADE'"
~~~

## 3. Usar el script `disable-flutter-tls-verification` de `NVISOsecurity`

~~~bash
frida -Uf com.app.name -l disable-flutter-tls.js
~~~
+ <a href="https://github.com/NVISOsecurity/disable-flutter-tls-verification" target="_blank">disable-flutter-tls-verification</a>

# Teoría y conceptos

## ¿Qué protecciones implementa Flutter?

### No pasa por el proxy del sistema
Dart (el motor runtime de Flutter) no utiliza la pila HTTP/HTTPS nativa del sistema operativo ni respeta la configuración de `Settings > Wi-Fi Proxy`. Implementa su propio cliente de red escrito en C/C++ utilizando BoringSSL, ignorando cualquier proxy configurado en la interfaz gráfica.

### Verificación TLS rígida
Las librerías compiladas de BoringSSL dentro del binario de Flutter (`libflutter.so`) no leen las autoridades de certificación (CAs) del almacenamiento de Android (ni User ni System). La verificación del handshake TLS se ejecuta directamente a nivel nativo en C/C++ mediante funciones internas como `ssl_crypto_x509_session_verify_cert` o `handshake_state`.

## ¿Cómo traspasar estas medidas de seguridad?

1. **Forzar el enrutamiento (Capa 3/4):** Mediante reglas de `iptables` a nivel de Kernel, se interceptan los paquetes salientes TCP (puertos 80 y 443) y se redirigen al socket de Burp Suite (configurado en modo Invisible Proxy), obligando a la aplicación a comunicarse con el proxy aunque lo ignore a nivel de aplicación.
2. **Bypass de la validación TLS (Capa de Aplicación/Memoria):** Se utiliza Frida para escanear en la memoria del proceso cargado (`libflutter.so`) los patrones de bytes (*signatures*) de las funciones nativas de verificación de BoringSSL, alterando su valor de retorno en tiempo de ejecución para forzar que la validación devuelva siempre éxito (`true` / `0x1` / `ssl_verify_ok`).

# ¿Lo buscabas para iOS?
+ <a href="https://elcaza.github.io/posts/hacking/flutter_ssl_pinning_ios" target="_blank">Deshabilitar la verificación TLS / SSL Pinning de Flutter (iOS)</a>

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