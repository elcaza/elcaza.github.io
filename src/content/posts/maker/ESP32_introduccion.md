---
title: Introducción al ESP32, consideraciones y referencia de pines
published: 2025-10-11
description: '¿Qué es el ESP32? ¿Qué pines debería utilizar en mi proyecto? ¿Cuáles pines debería evitar?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/esp32s.jpeg'
tags: [Maker]
category: 'Maker'
draft: false 
lang: 'es'
---

Cuando trabajamos con un ESP32 podríamos dudar sobre qué pines deberíamos usar en nuestro proyecto, cuál placa debería usar y si es mejor 38 o 30 pines. Este artículo pretende dar un breve resumen sobre lo que tenemos que tomar en cuenta.
+ Ojo, parte de las imagenes y tablas fueron tomadas de otros sitios. Al final del artículo podrán ver las fuentes.

# ¿Qué es el ESP32?

Una serie de microcontroladores desarrollados por Espressif.

# ¿Existen muchas versiones? ¿Cuál debería usar?

+ Sí, existen muchas versiones y estas variarán en caracteristicas y capacidades. Algunos son dual-core, otros son single-core, algunos tienen pantallas OLED incorporadas. 
+ Principalmente podrás encontrar placas con 30 y 38 pines expuestos.
+ Existen versiones con antenas WiFi externas e internas.
+ Comunmente trabajamos con "`ESP32 DEVKIT DOIT`". El equipo de "Random Nerds Tutorials" también recomienda el uso de:
	+ Adafruit ESP32 Feather
	+ Sparkfun ESP32 Thing
	+ NodeMCU-32S
	+ Wemos LoLin32
+ El ESP32-S3 puede ser una gran opción debido a sus capacidades mejoradas

# Consideraciones importantes al usar ESP32 DEVKIT DOIT

+ Para la conexión vía usb en Windows, tu ESP32 podría utilizar el chip "CP2102" o "CH340". Es importante conocer cuál es la que tú usas e instalar los drivers correspondientes. Linux y Mac no requieren la instalación de drivers adicionales.
+ La placa puede venir con puerto microusb o USB C
+ La placa tiene el botón "EN", para resetear el dispositivo.
+ La placa tiene el botón "BOOT", para poner el dispositivo en "flashing mode"

# ¿Cuántos pines tiene el ESP32?

48 pines con múltiples funciones. No todos los pines vienen expuestos debido a que no todos pueden/deben usarse. Usualmente encontrarás ESP32 de 38 y 30 pines.

## ESP32 30 PINES VS 38 PINES
La diferencia entre la placa con 30 y 38 pines es la siguiente: 
+ 6 PINES GPIO extras, los cuales no se recomienda utilizar debido a que son parte del SPI Flash integrado.
+ 1 PIN GPIO 0
+ 1 PIN GND

En el siguiente link tienes una tabla comparativa. La cual puedes usar para desarrollar tus proyectos.
+ <a href="https://docs.google.com/spreadsheets/d/1YNoNZgmX--Lw4E8IRFao4P5vAvbYQ9OI-MIKGRNjCKg/edit?usp=sharing" target="_blank">Tabla comparatia ESP32 38 PIN VS 30 PIN</a>

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_diagramas/ESP32_tabla_comparativa.png" width="100%">

En conclusión, es posible que no extrañes esos pines. 

# Diagrama de pines en el ESP32

## ¿Se puede ocupar cualquier PIN?

El ESP32 cuenta con la "función de multiplexación", la cual permite asignar el funcionamiento de cada GPIO. Por lo anterior, podrías asignar cualquier PIN para tu proyecto y el ESP32 haría las asignaciones correctas (aplican restricciones). Sin embargo, el estándar es el siguiente.

## Pines por defecto

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32_board.png" width="100%">

## Pines GPIO

Los Pines GPIO (General-Purpose Input/Output) son los pines que no tienen un propósito predefinido, sino que pueden ser configurados en tiempo de ejecución de dos maneras:
+ Modo de Salida (Output): El microcontrolador envía una señal digital (un estado lógico ALTO o BAJO). Esto se utiliza para controlar dispositivos externos, como encender un LED, activar un relé o controlar un motor.
+ Modo de Entrada (Input): El microcontrolador lee una señal digital de un dispositivo externo. Esto se utiliza para detectar estados, como si un botón está presionado, o leer datos de un sensor digital.

### Tabla de pines de uso seguro

<table >
	<tbody>
		<tr >
			<td>Etiqueta</td>
			<td>GPIO</td>
			<td>¿Es seguro de usar?</td>
			<td>Razón</td>
			<td>Ubicación por defecto</td>
		</tr>
		<tr>
			<td>D0</td>
			<td>0</td>
			<td><span >Evitarlo</span></td>
			<td>Must be HIGH during boot and LOW for programming</td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>TX0</td>
			<td>1</td>
			<td><span >NO</span></td>
			<td>Tx pin, used for flashing and debugging</td>
			<td></td>
		</tr>
		<tr>
			<td>D2</td>
			<td>2</td>
			<td><span >Evitarlo</span></td>
			<td>must be LOW during boot and also connected to the on-board LED</td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>RX0</td>
			<td>3</td>
			<td><span >NO</span></td>
			<td>Rx pin, used for flashing and debugging</td>
			<td></td>
		</tr>
		<tr>
			<td>D4</td>
			<td>4</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D5</td>
			<td>5</td>
			<td><span >Evitarlo</span></td>
			<td>must be HIGH during boot</td>
			<td></td>
		</tr>
		<tr>
			<td>D6</td>
			<td>6</td>
			<td><span >NO</span></td>
			<td>Connected to Flash memory</td>
			<td></td>
		</tr>
		<tr>
			<td>D7</td>
			<td>7</td>
			<td><span >NO</span></td>
			<td>Connected to Flash memory</td>
			<td></td>
		</tr>
		<tr>
			<td>D8</td>
			<td>8</td>
			<td><span >NO</span></td>
			<td>Connected to Flash memory</td>
			<td></td>
		</tr>
		<tr>
			<td>D9</td>
			<td>9</td>
			<td><span >NO</span></td>
			<td>Connected to Flash memory</td>
			<td></td>
		</tr>
		<tr>
			<td>D10</td>
			<td>10</td>
			<td><span >NO</span></td>
			<td>Connected to Flash memory</td>
			<td></td>
		</tr>
		<tr>
			<td>D11</td>
			<td>11</td>
			<td><span >NO</span></td>
			<td>Connected to Flash memory</td>
			<td></td>
		</tr>
		<tr>
			<td>D12</td>
			<td>12</td>
			<td><span >Evitarlo</span></td>
			<td>must be LOW during boot</td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D13</td>
			<td>13</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D14</td>
			<td>14</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D15</td>
			<td>15</td>
			<td><span >Evitarlo</span></td>
			<td>must be HIGH during boot, prevents startup log if pulled LOW</td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>RX2</td>
			<td>16</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>TX2</td>
			<td>17</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D18</td>
			<td>18</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D19</td>
			<td>19</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D21</td>
			<td>21</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D22</td>
			<td>22</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D23</td>
			<td>23</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Derecha</td>
		</tr>
		<tr>
			<td>D25</td>
			<td>25</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D26</td>
			<td>26</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D27</td>
			<td>27</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D32</td>
			<td>32</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D33</td>
			<td>33</td>
			<td><span >SÍ</span></td>
			<td></td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D34</td>
			<td>34</td>
			<td><span >Evitarlo</span></td>
			<td>Input only GPIO, cannot be configured as output</td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>D35</td>
			<td>35</td>
			<td><span >Evitarlo</span></td>
			<td>Input only GPIO, cannot be configured as output</td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>VP</td>
			<td>36</td>
			<td><span >Evitarlo</span></td>
			<td>Input only GPIO, cannot be configured as output</td>
			<td>Izquierda</td>
		</tr>
		<tr>
			<td>VN</td>
			<td>39</td>
			<td><span >Evitarlo</span></td>
			<td>Input only GPIO, cannot be configured as output</td>
			<td>Izquierda</td>
		</tr>
	</tbody>
</table>

## Pines de uso seguro

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-GPIO-Pins-that-are-Safe-to-Use.webp" width="100%">

## Pines I2C (Inter-Integrated Circuit)

Los pines I2C (Inter-Integrated Circuit) del ESP32 son los pines de comunicación serial síncrona que permiten al microcontrolador interactuar con dispositivos periféricos de baja velocidad. Pantallas OLED, sensores y módulos de expansión. 
+ Usan dos líneas para conectar varios dispositivos: SDA (Serial Data) y SCL (Serial Clock).
+ Su comunicación es Maestro/Esclavo
+ Cada dispositivo Esclavo en el bus debe tener una dirección única (generalmente de 7 bits), lo que permite al Maestro comunicarse con hasta 127 dispositivos diferentes en el mismo par de cables.
+ Requiere que los pines cuenten con resistencias pull-up (En el ESP32 casi todos los GPIO excepto los de solo entrada)

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-I2C-Pins.webp" width="100%">

## Pines SPI (Serial Peripheral Interface)

Los pines SPI (Serial Peripheral Interface) del ESP32 son un conjunto de líneas de comunicación serial síncrona de alta velocidad. Es capaz de transferir datos rápidamente entre el microcontrolador y periféricos como módulos de radio, tarjetas SD o pantallas grandes.

El ESP32 cuenta con dos controladores SPI: 
+ VSPI
+ HSPI

A diferencia de I2C, SPI utiliza cuatro líneas para operar. Esto permite la comunicación Full-Duplex (transmisión y recepción simultánea)
+ SCK - Serial Clock - Línea generada por el Maestro (ESP32) para sincronizar la transferencia de datos
+ MOSI - Master Out, Slave In - Línea de salida de datos del Maestro (ESP32) hacia el Esclavo (periférico)
+ MISO - Master In, Slave Out - Línea de entrada de datos al Maestro (ESP32) desde el Esclavo (periférico).
+ CS/SS - Chip Select (Slave Select) - Línea de habilitación que el Maestro usa para seleccionar (activar) el periférico Esclavo con el que quiere comunicarse

Pines SPI por defecto:


<table>
<thead>
	<tr>
		<td>Controlador</td>
		<td>Especificación</td>
		<td>Pin (ESP32)</td>
		<td>2da opción *</td>
		<td>Ubicación</td>
	</tr>
</thead>
<tbody>
	<tr>
		<td>VSPI (Virtual SPI / SPI2)</td>
		<td>&#128993; CE</td>
		<td>NA</td>
		<td>GPIO 04</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>VSPI (Virtual SPI / SPI2)</td>
		<td>&#128992; CSN</td>
		<td>GPIO 05</td>
		<td>Derecha</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>VSPI (Virtual SPI / SPI2)</td>
		<td>&#128994; SCK</td>
		<td>GPIO 18</td>
		<td>Derecha</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>VSPI (Virtual SPI / SPI2)</td>
		<td>&#128998; MOSI</td>
		<td>GPIO 23</td>
		<td>Derecha</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>VSPI (Virtual SPI / SPI2)</td>
		<td>&#128995; MISO</td>
		<td>GPIO 19</td>
		<td>Derecha</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>VSPI (Virtual SPI / SPI2)</td>
		<td>IRQ</td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td>HSPI (Hardware SPI / SPI3)</td>
		<td>&#128993; CE</td>
		<td>NA</td>
		<td>GPIO 02</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>HSPI (Hardware SPI / SPI3)</td>
		<td>&#128992; CSN</td>
		<td>GPIO 15</td>
		<td>Derecha</td>
		<td>Derecha</td>
	</tr>
	<tr>
		<td>HSPI (Hardware SPI / SPI3)</td>
		<td>&#128994; SCK</td>
		<td>GPIO 14</td>
		<td>Izquierda</td>
		<td>Izquierda</td>
	</tr>
	<tr>
		<td>HSPI (Hardware SPI / SPI3)</td>
		<td>&#128998; MOSI</td>
		<td>GPIO 13</td>
		<td>Izquierda</td>
		<td>Izquierda</td>
	</tr>
	<tr>
		<td>HSPI (Hardware SPI / SPI3)</td>
		<td>&#128995; MISO</td>
		<td>GPIO 12</td>
		<td>Izquierda</td>
		<td>Izquierda</td>
	</tr>
	<tr>
		<td>HSPI (Hardware SPI / SPI3)</td>
		<td>IRQ</td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
</tbody>
</table>

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-SPI-Pins.webp" width="100%">

## Pines ADC (Conversor Analógico-Digital)

Los pines ADC (Analog-to-Digital Converter) del ESP32 son los pines GPIO que pueden medir voltajes analógicos continuos y convertirlos en valores digitales discretos. Esto permite al ESP32 leer datos de sensores analógicos, como termistores, potenciómetros o sensores de luz.
+ ADC1 - Puede usarse libremente en cualquier momento.
+ ADC2 - Podría generar conflictos, pues se comparte con la funcionalidad WiFi del ESP32.

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-ADC-Pins.webp" width="100%">

## Pines DAC (Convertidor Digital a Analógico) 

Los pines DAC (Digital-to-Analog Converter) del ESP32 son los pines GPIO que pueden convertir un valor digital (un número, generalmente de 0 a 255) en un voltaje analógico de salida correspondiente. Esto permite al ESP32 generar señales de audio simples o formas de onda analógicas arbitrarias sin necesidad de componentes externos.

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-DAC-Pins.webp" width="100%">

## Pines Touch

Los pines Touch del ESP32 son pines GPIO especiales que pueden detectar cambios en la capacitancia eléctrica, lo que permite utilizarlos como sensores táctiles sin la necesidad de componentes adicionales. Cuando un dedo toca un área conductora conectada al pin, el sistema detecta un cambio.

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-Touch-Pins.webp" width="100%">

## Pines UART (Universal Asynchronous Receiver/Transmitter)

Los pines UART (Universal Asynchronous Receiver/Transmitter) del ESP32 son los pines de comunicación serial asíncrona que permiten al microcontrolador enviar y recibir datos en serie a otros dispositivos (como una PC a través de USB/Serial o módulos Bluetooth) utilizando un formato simple de inicio/datos/parada. 
+ Se debe tener extremo cuidado al usar los pines UART que coinciden con los del bus SPI Flash (GPIO 9, 10, 11)

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-UART-Pins.webp" width="100%">

## Pines RTC (Real Time Clock)

Los pines RTC (Real-Time Clock) del ESP32 son un subconjunto de pines GPIO que están conectados al dominio de energía RTC low-power. Estos pines tienen la capacidad única de mantener su estado y funcionalidad de despertar (wake-up) mientras el chip está en modo de Sueño Profundo (Deep Sleep). 

El dominio RTC es crucial para las aplicaciones de muy bajo consumo en el ESP32, ya que permite que la mayor parte del chip permanezca apagada mientras una pequeña porción sigue monitoreando los eventos.

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-RTC-GPIO-Pins.webp" width="100%">

## Pines Strapping (Sujeción)

Los pines Strapping (o pines de configuración de arranque) del ESP32 son un pequeño grupo de pines GPIO cuya condición (si están en estado ALTO/High o BAJO/Low) es leída por el chip durante el proceso de reinicio o encendido. Su estado dicta el modo de arranque del ESP32 para saber si se tiene que:
+ ejecutar el programa ya cargado
+ esperar a recibir un nuevo firmware. 

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-Strapping-Pins.webp" width="100%">

## Pines de energía

Los Pines de Energía del ESP32 son los pines dedicados a alimentar el microcontrolador y el resto de la placa, así como a proporcionar puntos de conexión a tierra. 
+ **5V (V-IN o VBUS)**: Este pin generalmente se conecta a la entrada de alimentación de 5 voltios, ya sea directamente desde el puerto USB o desde una fuente externa. Si alimentas el ESP32 a través de este pin, el regulador de voltaje de la placa reduce esta tensión a 3.3V para el chip.
+ **3V3 (o VDD)**: Este pin proporciona la tensión de operación nativa del chip ESP32, que es de 3.3 voltios. Puedes usar este pin para:
	+ Alimentar el ESP32 directamente (si omites el regulador de 5V).
	+ Suministrar energía a periféricos externos de baja potencia (como sensores o módulos) que operan a 3.3V.
+ **GND (Tierra)**: Es el pin de referencia de potencial eléctrico (0 voltios).

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-Power-Pins.webp" width="100%">

## Enable Pin

El pin EN (Enable) es un pin de control, aunque está íntimamente ligado a la alimentación, ya que controla la activación del chip:
+ Función: Cuando el pin EN está conectado a ALTO (3.3V), el chip ESP32 está habilitado y funcionando.
+ Reinicio: Conectar el pin EN a BAJO (GND) realiza un reinicio físico (hardware reset) del chip. Este pin generalmente está conectado al botón de "Reset" o "RST" de la placa.

A diferencia de un reinicio por software (esp_restart()), usar el pin EN es una forma de garantizar que el chip se reinicie desde cero.

El uso de la función esp_restart() en el ESP32 no garantiza un reinicio "desde cero" porque realiza un reinicio de tipo software, no un reinicio físico (hardware).

<table>
<thead>
	<tr>
		<td>Tipo de Reinicio</td>
		<td>Mecanismo</td>
		<td>Registros/Memoria RTC</td>
		<td>Garantía de "Desde Cero"</td>
	</tr>
</thead>
<tbody>
	<tr>
		<td>Software ( esp_restart() )</td>
		<td>El código salta a la rutina de inicio.</td>
		<td>Persisten</td>
		<td>No (Solo reinicia el núcleo principal)</td>
	</tr>
	<tr>
		<td>Hardware (Botón EN/RST)</td>
		<td>El voltaje de alimentación se desconecta/se corta temporalmente.</td>
		<td>Se borran</td>
		<td>Sí (Limpia todo el estado del chip)</td>
	</tr>
</tbody>
</table>

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_introduccion/ESP32-Enable-Pin.webp" width="100%">

# Referencias y más información

Thanks a lot for your content and images.
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