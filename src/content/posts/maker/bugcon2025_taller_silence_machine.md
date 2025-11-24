---
title: BugCon 2025, taller Silence Machine
published: 2025-11-24
description: 'Todo el material necesario para crear tu propia máquina generadora de silencio.'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/bugcon2025_taller_silence_machine/portada.jpeg'
tags: [Maker, silence_machine]
category: 'Maker'
draft: false 
lang: 'es'
---

Durante la BugCon 2025, tuve el placer de colaborar con la Villa RF y presentar a "La máquina generadora de silencio", un dispositivo que te ayuda a recobrar la paz perdida cuando la bocina ruidosa de tu vecino suena a máximo volumen. 
 
Si te gustaría saber más sobre el proyecto, te recomiendo leer el artículo presentación de <a href="https://elcaza.github.io/posts/maker/silence_machine/" target="_blank">la máquina generadora de silencio</a>.

En este artículo abordaremos paso a paso la creación de la máquina generadora de silencio.

# Presentación compartida durante la BugCon
+ <a href="https://drive.google.com/drive/folders/1whjk0r1jHvVh9JrQjt0LCzw3TOpWxE8V?usp=drive_link" target="_blank">Presentación</a>

# Materiales requeridos para el taller

## Materiales requeridos para crear la máquina generadora de silencio

+ 1 ESP32 o similar. 30 o 38 pines, son bienvenidos.
+ 2 módulos de radio a 2.4 Ghz
+ 1 Pantala OLED con 4 botones
+ 2 Capacitores
+ 1 Protoboard
+ Cables dupont
	+ 16 Female Female (1 PAQUETE DE 40, puede dar dos juegos.  Sobran conexiones rojas y negras)
		+ 2 AMARILLO 🟡
		+ 2 NARANJA 🟠
		+ 2 VERDE 🟢
		+ 2 AZUL 🔵
		+ 2 MORADO 🟣
		+ 2 BLANCO ⚪
		+ 2 GRIS 🔘
		+ 2 CAFE 🟤
	+ 8 Male Female (1 PAQUETE DE 40, puede dar un solo juego. Sobran todos los colores. Se recomienda suplir con café y blanco)
		+ 4 ROJO 🔴 || CAFE 🟤
		+ 4 NEGRO ⬛ || BLANCO ⚪

## Diagrama ESP32 

### Diagrama ESP32 30 pines
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_diagramas/ESP32_30_pin.png" width="100%">

### Diagrama ESP32 38 pines
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/esp32_diagramas/ESP32_38_pin.png" width="100%">

## Diagrama nRF24L01 PA LNA
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/rf_diagramas/nrf24l01_pa_lna.png" width="100%">

## Tabla de conexiones

<table>
	<caption>Silence Machine - ESP32</caption>
	<thead>
		<tr>
			<th>Componente</th>
			<th>Especificación</th>
			<th>Pin (ESP32)</th>
			<th>10uf capacitor / 2da opción</th>
			<th>Cable</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1st nRF24L01</td>
			<td>VCC 🔴</td>
			<td>3.3V</td>
			<td>N/A (+) capacitor</td>
			<td>Rojo</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>GND ⬛</td>
			<td>GND</td>
			<td>N/A (-) capacitor</td>
			<td>Negro</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>CE 🟡</td>
			<td>GPIO 04</td>
			<td></td>
			<td>Amarillo</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>CSN 🟠</td>
			<td>GPIO 05</td>
			<td></td>
			<td>Naranja</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>SCK 🟢</td>
			<td>GPIO 18</td>
			<td></td>
			<td>Verde</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>MOSI 🔵</td>
			<td>GPIO 23</td>
			<td></td>
			<td>Azul</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>MISO 🟣</td>
			<td>GPIO 19</td>
			<td></td>
			<td>Morado</td>
		</tr>
		<tr>
			<td>1st nRF24L01</td>
			<td>IRQ</td>
			<td></td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>VCC 🔴</td>
			<td>3.3V</td>
			<td>N/A (+) capacitor</td>
			<td>Rojo</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>GND ⬛</td>
			<td>GND</td>
			<td>N/A (-) capacitor</td>
			<td>Negro</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>CE 🟡</td>
			<td>GPIO 02</td>
			<td></td>
			<td>Amarillo</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>CSN 🟠</td>
			<td>GPIO 15</td>
			<td></td>
			<td>Naranja</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>SCK 🟢</td>
			<td>GPIO 14</td>
			<td></td>
			<td>Verde</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>MOSI 🔵</td>
			<td>GPIO 13</td>
			<td></td>
			<td>Azul</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>MISO 🟣</td>
			<td>GPIO 12</td>
			<td></td>
			<td>Morado</td>
		</tr>
		<tr>
			<td>2do nRF24L01</td>
			<td>IRQ</td>
			<td></td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>Button 1 - UP</td>
			<td>Terminal</td>
			<td>GPIO 25</td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>Button 2 - DOWN</td>
			<td>Terminal</td>
			<td>GPIO 26</td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>Button 3 - #</td>
			<td>Terminal</td>
			<td>EN</td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>Button 4 - *</td>
			<td>Terminal</td>
			<td>GPIO 27</td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>Display 1</td>
			<td>VCC 🔴</td>
			<td>3.3V</td>
			<td></td>
			<td>Rojo</td>
		</tr>
		<tr>
			<td>Display 1</td>
			<td>GND ⬛</td>
			<td>GND</td>
			<td></td>
			<td>Negro</td>
		</tr>
		<tr>
			<td>Display 1</td>
			<td>SDA 🟡</td>
			<td>GPIO 21</td>
			<td>GPIO 32</td>
			<td>Amarillo</td>
		</tr>
		<tr>
			<td>Display 1</td>
			<td>SCL 🟠</td>
			<td>GPIO 22</td>
			<td>GPIO 33</td>
			<td>Naranja</td>
		</tr>
	</tbody>
</table>

+ Las otras terminales de los botones, se conectan a GND. Cuando se usa la pantalla OLED con botones integrados, estos ya están cableados a GND.
+ Si se ocupan los pines de 2da opción, se debe modificar el código correspondiente en `// INICIO DE OPCIONES CONFIGURABLES`

# Instrucciones de armado

## 1) Cableado de los 2 módulos nRF24L01

Una los cables para cada módulo nRF24L01. Tenga en cuenta lo siguiente:
+ Los cables deben coincidir con la posición del módulo (vea la imagen adjunta)
+ Positivo y negativo son cables macho-hembra.
+ Este módulo solamente se alimenta con 3.3v, si se conecta a 5v se quema

## 2) Cableado de la pantalla OLED de 4 botones

Una los cables para el módulo OLED de 4 botones. Tenga en cuenta lo siguiente:
+ La pantalla indica qué es cada pin
+ Positivo y negativo son cables macho-hembra.
+ Este módulo solamente se alimenta con 3.3v

## 3) Conecte el primer módulo RF a la ESP32
Consideraciones:
1. Verifique que el módulo RF este correctamente cableado
1. Los pines de alimentación van directo a la protoboard (paso final)
1. Las imagenes del ESP32 (30 O 38 pins) son ilustrativos, pero deben coincidir con los de su board

## 4) Conecte el segundo módulo RF a la ESP32
Consideraciones:
1. Verifique que el módulo RF este correctamente cableado
1. Los pines de alimentación van directo a la protoboard (paso final)
1. Las imagenes del ESP32 (30 O 38 pins) son ilustrativos, pero deben coincidir con los de su board

## 5) Conecte la pantalla OLED de 4 botones
Consideraciones:
1. Verifique que la pantalla este correctamente cableada
1. Los pines de alimentación van directo a la protoboard (paso final)
1. Las imagenes del ESP32 (30 O 38 pins) son ilustrativos, pero deben coincidir con los de su board

## 6) Prepare la protoboard de alimentación
Usando una protoboard, use las líneas positivo (rojo) y negativo (azul) para:
1. Alimentar a 3.3v desde el protoboard. **A 3.3v, si usas 5v se van a quemar los módulos RF**
1. Poner un capacitor, positivo en positivo y negativo en negativo. **Si se conectan al revés explota el capacitor**
1. Conectar el primer módulo RF. 
1. Poner un capacitor, positivo en positivo y negativo en negativo. 
1. Conectar el segundo módulo RF. 
1. Conectar la pantalla OLED

## 7) Carga de firmware
1. <a href="https://elcaza.github.io/posts/maker/arduino_esp32/" target="_blank">Tutorial para instalar Arduino IDE y las boards necesarias para el ESP32</a>
1. <a href="https://github.com/elcaza/silence_machine" target="_blank">Descargue el código para la Silence Machine</a>
	+ La versión dual core es la recomendada, pues esta tiene un mejor rendimiento
1. Abra Arduino IDE, conecte su ESP32 y cargue el código
1. Ahora tiene su Silence Machine lista

## 8) Modo de operación
1. Prende el dispositivo
1. Se corre de forma automática un chequeo de salud, se muestra el resultado en la pantalla
	+ Si todo sale bien, puedes proseguir 
	+ Si alguno de los módulos RF falla, el resultado se mostrara en pantalla
	+ Si la pantalla falla, en el Serial se mostrará el error
	+ En muchas ocasiones, solo con reiniciarlo, `botón #`, se corregirá el error
		+ Si no se soluciona, revise el cableado
1. Seleccionas
	+ `#` => para reiniciar
	+ `*` => para continuar
1. Seleccionas un modo y generas silencio
	+ `^` => arriba
	+ `v` => abajo
	+ `#` => reiniciar
	+ `*` => continuar
1. Una vez iniciada la generación de silencio, solo puedes parar mediante el botón
	+ `#` => reiniciar

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