---
title: Deshabilitar la verificación TLS / SSL Pinning de Flutter (iOS)
published: 2026-08-23
description: 'Deshabilitar el SSL Pinning de Flutter y Dart en iOS'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/ios_android_flutter_ssl_pinning/portada.jpeg'
tags: [Hacking, iOS]
category: 'Hacking'
draft: false 
lang: 'es'
---

# Deshabilitar las protecciones TLS en aplicativos hechos con Flutter

Las aplicaciones desarrolladas con Flutter/Dart evaden los métodos tradicionales de intercepción de tráfico. Al ignorar el proxy configurado en los ajustes de iOS y descartar las CAs instaladas en el almacenamiento del sistema operativo (User/System), la interceptación mediante Burp Suite falla debido a que las peticiones nunca llegan al proxy. Este documento detalla el procedimiento para forzar el enrutamiento a nivel de Kernel (`iptables`) en un Gateway intermedio y neutralizar la validación TLS en memoria mediante instrumentación dinámica (`Frida`).

# Just do it - Breve resumen de la solución

## 0. Requisitos:
+ Proxy en modo invisible
+ Sistema en modo gateway, un Linux con acceso a iptables (Debian 12 en mi caso)
+ Celular iOS
  + iPhone 8 - iOS 16.7.11 - Frida 16.5.2
+ Script `disable-flutter-tls-verification` de `NVISOsecurity`
  + <a href="https://github.com/NVISOsecurity/disable-flutter-tls-verification" target="_blank">disable-flutter-tls-verification</a>

## 1. Proxy en modo invisible
Configuras Burp en modo "Invisible proxy" y escuchando en una interfaz
1. Proxy => HTTP Proxy => Proxy Settings => Proxy listeners => Interface Edit => Request handling => **Support invisible proxying** 
   1. La interfaz NO debe ser localhost

## 2. Sistema en modo gateway
Configurar a tu computadora para que sirva como gateway y realice el reenvio de peticiones

### Ver reglas de iptables actuales

~~~bash
sudo iptables -t nat -L PREROUTING -n -v --line-numbers && sudo iptables -t nat -L POSTROUTING -n -v --line-numbers && sudo iptables -L FORWARD -n -v --line-numbers
~~~

### Opción 1: Celular <=> Gateway y proxy (misma computadora)

Variables a configurar
~~~bash
INTERFACE=wlp2s0
PROXY_PORT=8888
~~~

Para crear las reglas
~~~bash
# Redirige el tráfico HTTP y HTTPS que llega a Debian hacia el puerto local de Burp Suite
sudo iptables -t nat -A PREROUTING -i $INTERFACE -p tcp --dport 80 -j REDIRECT --to-port $PROXY_PORT
sudo iptables -t nat -A PREROUTING -i $INTERFACE -p tcp --dport 443 -j REDIRECT --to-port $PROXY_PORT
~~~

Para remover las reglas
~~~bash
# Remover: Redirige el tráfico HTTP y HTTPS que llega a Debian hacia el puerto local de Burp Suite
sudo iptables -t nat -D PREROUTING -i $INTERFACE -p tcp --dport 80 -j REDIRECT --to-port $PROXY_PORT
sudo iptables -t nat -D PREROUTING -i $INTERFACE -p tcp --dport 443 -j REDIRECT --to-port $PROXY_PORT
~~~

### Opción 2: Celular <=> Gateway <=> Proxy

Variables a configurar
~~~bash
INTERFACE=wlp2s0
PROXY_IP=10.10.10.5
PROXY_PORT=8888
~~~

Para crear las reglas
~~~bash
# Redirige el tráfico HTTP y HTTPS que llega al Debian hacia el puerto de Burp en Windows
sudo iptables -t nat -A PREROUTING -i $INTERFACE -p tcp --dport 80 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT
sudo iptables -t nat -A PREROUTING -i $INTERFACE -p tcp --dport 443 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT

# Permite que las respuestas de Windows regresen al iPhone a través de Debian
sudo iptables -t nat -A POSTROUTING -o $INTERFACE -d $PROXY_IP -p tcp --dport $PROXY_PORT -j MASQUERADE
sudo iptables -A FORWARD -i $INTERFACE -o $INTERFACE -p tcp --dport $PROXY_PORT -j ACCEPT
~~~

Para remover las reglas
~~~bash
# Remover: Redirige el tráfico HTTP y HTTPS que llega al Debian hacia el puerto de Burp en Windows
sudo iptables -t nat -D PREROUTING -i $INTERFACE -p tcp --dport 80 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT
sudo iptables -t nat -D PREROUTING -i $INTERFACE -p tcp --dport 443 -j DNAT --to-destination $PROXY_IP:$PROXY_PORT

# Remover Permite que las respuestas de Windows regresen al iPhone a través de Debian
sudo iptables -t nat -D POSTROUTING -o $INTERFACE -d $PROXY_IP -p tcp --dport $PROXY_PORT -j MASQUERADE
sudo iptables -D FORWARD -i $INTERFACE -o $INTERFACE -p tcp --dport $PROXY_PORT -j ACCEPT
~~~

## 3. Configura tu celular conexión WiFi en modo manual

### iOS

Seleccionas la red WiFi donde vivan tus dispositivos
1. Configure IP => Manual
2. IP Address => Una de tu segmento (ej. 10.10.10.222)
3. Subnet Mask => La de tu segmento, comunmente 255.255.255.0
4. Router => La IP del Linux que va a servir de puente (ej. 10.10.10.3)
5. Configure DNS => Manual (Añadir servidor 1.1.1.1 u 8.8.8.8)

Nota: Reconectar dispositivo (Apagar y encender WiFi del celular en caso de que sea necesario)

## 4. Usar el script `disable-flutter-tls-verification` de `NVISOsecurity`

~~~bash
frida -Uf com.app.name -l disable-flutter-tls.js
~~~
+ <a href="https://github.com/NVISOsecurity/disable-flutter-tls-verification" target="_blank">disable-flutter-tls-verification</a>

# Teoría y conceptos

## ¿Qué protecciones implementa Flutter?

### No pasa por el proxy del sistema
Dart (el motor runtime de Flutter) no utiliza la pila HTTP/HTTPS nativa del sistema operativo ni respeta la configuración de `Settings > Wi-Fi Proxy`. Implementa su propio cliente de red escrito en C/C++ utilizando BoringSSL, ignorando cualquier proxy configurado en la interfaz gráfica.

### Verificación TLS rígida
Las librerías compiladas de BoringSSL dentro del motor de Flutter (`Flutter.framework` o el binario ejecutable en iOS) no leen las autoridades de certificación (CAs) del almacenamiento del sistema operativo. La verificación del handshake TLS se ejecuta directamente a nivel nativo en C/C++ mediante funciones internas como `ssl_crypto_x509_session_verify_cert` o `handshake_state`.

## ¿Cómo traspasar estas medidas de seguridad?

1. **Forzar el enrutamiento (Capa 3/4):** Mediante reglas de `iptables` a nivel de Kernel en un Gateway, se interceptan los paquetes salientes TCP (puertos 80 y 443) provenientes del iPhone y se redirigen al socket de Burp Suite (configurado en modo Invisible Proxy), obligando a la aplicación a comunicarse con el proxy aunque lo ignore a nivel de aplicación.
2. **Bypass de la validación TLS (Capa de Aplicación/Memoria):** Se utiliza Frida para escanear en la memoria del proceso cargado (específicamente en el motor de Flutter compilado) los patrones de bytes (*signatures*) de las funciones nativas de verificación de BoringSSL, alterando su valor de retorno en tiempo de ejecución para forzar que la validación devuelva siempre éxito (`true` / `0x1` / `ssl_verify_ok`).

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