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


Cuando trabajamos con un ESP32 podríamos dudar sobre qué pines deberíamos usar en nuestro proyecto, cuál placa debería usar y si es mejor 38 o 30 pines. Este artículo pretende dar un breve resumen sobre lo que tenemos que tomar en cuenta.

# ¿Qué es el ESP32?
Una serie de microcontroladores desarrollados por Espressif.

# ¿Existen muchas versiones? ¿Cuál debería usar?
+ Sí existen muchas versiones. Estas variarán en caracteristicas y capacidades. Algunos son dual-core, otros de un solo núcleo, algunos tienen pantallas OLED incorporadas. 
+ Principalmente podrás encontrar placas con 30 y 38 pines expuestos.
+ Existen versiones con antenas WiFi externas e internas.
+ Comunmente trabajamos con "`ESP32 DEVKIT DOIT`". El equipo de "Random Nerds Tutorials" también recomienda el uso de:
	+ Adafruit ESP32 Feather
	+ Sparkfun ESP32 Thing
	+ NodeMCU-32S
	+ Wemos LoLin32

# Consideraciones importantes al usar ESP32 DEVKIT DOIT
+ Para la conexión vía usb en Windows, tu ESP32 podría utilizar el chip "CP2102" o "CH340". Es importante conocer cuál es la que tú usas e instalar los drivers correspondientes.
+ Las placas pueden venir con puerto microusb o USB C
+ Las placas tiene el botón "EN", para resetear el dispositivo.
+ Las placas tiene el botón "BOOT", para poner el dispositivo en "flashing mode"

# ¿Cuántos pines tiene el ESP32?
48 pines con múltiples funciones. No todos los pines vienen expuestos debido a que no todos pueden usarse. Usualmente encontrarás ESP32 de 38 y 30 pines.

## ESP32 30 PINES VS 38 PINES
La diferencia entre la placa con 30 y 38 pines es la siguiente: 
+ 6 PINES GPIO extras, los cuales no se recomienda utilizar debido a que son parte del SPI Flash integrado.
+ 1 PIN GPIO 0
+ 1 PIN GND

En conclusión, es posible que para la mayor parte de tus proyectos no extrañes esos pines. A continuación, se muestra lo explicado.
 
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32_board.png" width="100%">
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32_board.png" width="100%">


# ¿Se puede ocupar cualquier PIN?
El ESP32 cuenta con la "función de multiplexación", la cual permite asignar el funcionamiento de cada GPIO. Sin embargo, debes tomar en cuenta lo siguiente:

## Pines por defecto

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/introduccion_esp32/ESP32_board.png" width="100%">

## Pines GPIO

### Pines de uso seguro

<table >
	   <tbody>
		  <tr >
			 <td>&nbsp;&nbsp;Label&nbsp;&nbsp;</td>
			 <td>&nbsp;&nbsp;GPIO&nbsp;&nbsp;</td>
			 <td>&nbsp;&nbsp;Safe&nbsp;to&nbsp;use?&nbsp;&nbsp;</td>
			 <td>Reason</td>
		  </tr>
		  <tr>
			 <td>D0</td>
			 <td>0</td>
			 <td><span >SI</span></td>
			 <td>must be HIGH during boot and LOW for programming</td>
		  </tr>
		  <tr>
			 <td>TX0</td>
			 <td>1</td>
			 <td><span >SI</span></td>
			 <td>Tx pin, used for flashing and debugging</td>
		  </tr>
		  <tr>
			 <td>D2</td>
			 <td>2</td>
			 <td><span >SI</span></td>
			 <td>must be LOW during boot and also connected to the on-board LED</td>
		  </tr>
		  <tr>
			 <td>RX0</td>
			 <td>3</td>
			 <td><span >SI</span></td>
			 <td>Rx pin, used for flashing and debugging</td>
		  </tr>
		  <tr>
			 <td>D4</td>
			 <td>4</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D5</td>
			 <td>5</td>
			 <td><span >SI</span></td>
			 <td>must be HIGH during boot</td>
		  </tr>
		  <tr>
			 <td>D6</td>
			 <td>6</td>
			 <td><span >SI</span></td>
			 <td>Connected to Flash memory</td>
		  </tr>
		  <tr>
			 <td>D7</td>
			 <td>7</td>
			 <td><span >SI</span></td>
			 <td>Connected to Flash memory</td>
		  </tr>
		  <tr>
			 <td>D8</td>
			 <td>8</td>
			 <td><span >SI</span></td>
			 <td>Connected to Flash memory</td>
		  </tr>
		  <tr>
			 <td>D9</td>
			 <td>9</td>
			 <td><span >SI</span></td>
			 <td>Connected to Flash memory</td>
		  </tr>
		  <tr>
			 <td>D10</td>
			 <td>10</td>
			 <td><span >SI</span></td>
			 <td>Connected to Flash memory</td>
		  </tr>
		  <tr>
			 <td>D11</td>
			 <td>11</td>
			 <td><span >SI</span></td>
			 <td>Connected to Flash memory</td>
		  </tr>
		  <tr>
			 <td>D12</td>
			 <td>12</td>
			 <td><span >SI</span></td>
			 <td>must be LOW during boot</td>
		  </tr>
		  <tr>
			 <td>D13</td>
			 <td>13</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D14</td>
			 <td>14</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D15</td>
			 <td>15</td>
			 <td><span >SI</span></td>
			 <td>must be HIGH during boot, prevents startup log if pulled LOW </td>
		  </tr>
		  <tr>
			 <td>RX2</td>
			 <td>16</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>TX2</td>
			 <td>17</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D18</td>
			 <td>18</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D19</td>
			 <td>19</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D21</td>
			 <td>21</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D22</td>
			 <td>22</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D23</td>
			 <td>23</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D25</td>
			 <td>25</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D26</td>
			 <td>26</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D27</td>
			 <td>27</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D32</td>
			 <td>32</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D33</td>
			 <td>33</td>
			 <td><span >SI</span></td>
			 <td></td>
		  </tr>
		  <tr>
			 <td>D34</td>
			 <td>34</td>
			 <td><span >SI</span></td>
			 <td>Input only GPIO, cannot be configured as output</td>
		  </tr>
		  <tr>
			 <td>D35</td>
			 <td>35</td>
			 <td><span >SI</span></td>
			 <td>Input only GPIO, cannot be configured as output</td>
		  </tr>
		  <tr>
			 <td>VP</td>
			 <td>36</td>
			 <td><span >SI</span></td>
			 <td>Input only GPIO, cannot be configured as output</td>
		  </tr>
		  <tr>
			 <td>VN</td>
			 <td>39</td>
			 <td><span >SI</span></td>
			 <td>Input only GPIO, cannot be configured as output</td>
		  </tr>
	   </tbody>
	</table>

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