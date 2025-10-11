---
title: Introducción al ESP32 - Pinout Reference
published: 2025-10-11
description: '¿Qué es el ESP32? ¿Qué pines debería utilizar en mi proyecto? ¿Cuáles pines debería evitar?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/windows/kms/como_funcionan_los_activadores_kms.png'
tags: [Maker]
category: 'Maker'
draft: false 
lang: 'es'
---

** Aún no terminado 


Cuando trabajamos con un ESP32 podríamos dudar sobre qué pines deberíamos usar en nuestro proyecto. Este artículo pretende dar un breve resumen sobre lo que se debe tomar en cuenta.

# ¿Qué es el ESP32?
Una serie de microcontroladores desarrollados por Espressif.

# ¿Existen muchas versiones? ¿Cuál debería usar?
+ Sí existen muchas versiones. Estas variarán en caracteristicas y capacidades.
+ Principalmente podrás encontrar placas con 30 y 38 pines expuestos.
+ Comunmente trabajamos con "`ESP32 DEVKIT DOIT`". La gente de "Random Nerds Tutorials" también recomiendan:
	+ Adafruit ESP32 Feather
	+ Sparkfun ESP32 Thing
	+ NodeMCU-32S
	+ Wemos LoLin32

# Consideraciones importantes al usar ESP32 DEVKIT DOIT
+ Para la conexión vía usb con tu computadora podría utilizar el chip "CP2102" o "CH340". Es importante conocer cuál es la que tú usas debido a que son los drivers que deberás instalar.
+ Las placas pueden venir con puerto microusb o USB C
+ Las placas tiene el botón "EN", para resetear el dispositivo.
+ Las placas tiene el botón "BOOT", para poner el dispositivo en "flashing mode"

# ¿Cuántos pines tiene el ESP32?
48 pines con múltiples funciones. No todos los pines vienen expuestos debido a que no todos pueden usarse.

# ¿Se puede ocupar cualquier PIN?
El ESP32 cuenta con la "función de multiplexación", la cual permite asignar qué hará cada GPIO. Sin embargo, debes tomar en cuenta lo siguiente:

## Pines por defecto
ESP32-GPIO-Pins.webp
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32_board.png" width="100%">

## Pines GPIO

### Pines de uso seguro

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-GPIO-Pins-that-are-Safe-to-Use.webp" width="100%">

## Pines ADC (Conversor Analógico-Digital)

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-ADC-Pins.webp" width="100%">

## Pines DAC (Convertidor Digital a Analógico) 

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-DAC-Pins.webp" width="100%">

## Pines Touch

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-Touch-Pins.webp" width="100%">

## Pines I2C (Inter-Integrated Circuit)
Usan dos líneas para conectar varios dispositivos: SDA (Serial Data) y SCL (Serial Clock).

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-I2C-Pins.webp" width="100%">

## Pines SPI (Serial Peripheral Interface)
Se utilizan para la comunicación serie de alta velocidad con otros dispositivos

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-SPI-Pins.webp" width="100%">

## Pines UART (Universal Asynchronous Receiver/Transmitter)
A diferencia de otros protocolos como SPI o I2C, el UART no necesita una señal de reloj sincronizada. En su lugar, ambos dispositivos deben estar configurados a la misma velocidad de transmisión (baud rate). 

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-UART-Pins.webp" width="100%">

## Pines RTC GPIO (Real Time Clock)

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-RTC-GPIO-Pins.webp" width="100%">

## Pines Strapping (Sujeción)

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-Strapping-Pins.webp" width="100%">

## Pines de energía

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-Power-Pins.webp" width="100%">

## Enable Pin

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32-Enable-Pin.web" width="100%">

# Referencias y más información
+ <a href="https://randomnerdtutorials.com/esp32-pinout-reference-gpios/" target="_blank">ESP32 Pinout Reference: Which GPIO pins should you use?</a>
+ <a href="https://lastminuteengineers.com/esp32-pinout-reference/" target="_blank">ESP32 Pinout Reference</a>
+ <a href="https://randomnerdtutorials.com/getting-started-with-esp32/" target="_blank">Getting Started with the ESP32 Development Board</a>


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