---
title: La máquina generadora de silencio
published: 2025-11-17
description: 'La máquina generadora de silencio es un dispositivo que te ayuda a recobrar la paz perdida cuando la bocina ruidosa de tu vecino suena a máximo volumen.'
image: 'https://raw.githubusercontent.com/elcaza/silence_machine/refs/heads/main/silence_machine.jpeg'
tags: [Maker, silence_machine]
category: 'Maker'
draft: false 
lang: 'es'
---

# ¿Qué es la máquina generadora de silencio (Silence Machine)? 
La máquina generadora de silencio es un dispositivo que te ayuda a recobrar la paz perdida cuando la bocina ruidosa de tu vecino suena a máximo volumen. 
Existe la versión de un sólo núcleo y dos núcleos:
+ <a href="https://github.com/elcaza/silence_machine" target="_blank">Silence Machine - Dual Core (Recomendada)</a>
+ <a href="https://github.com/elcaza/silence_machine_single_core" target="_blank">Silence Machine - Single Core</a>
	+ La versión de un sólo núcleo es menos eficiente que la versión de dos núcleos, sin embargo cuenta con código más fácil de leer y programar.
	+ La versión de un sólo núcleo fue la prueba de concepto para crear la versiń dual core.

Para entender su funcionamiento necesitamos entender lo siguiente:
1. La música es la combinación de sonidos, ritmos y silencios expresados en ondas y frecuencias.
1. Pese a las reglamentaciones locales, las autoridades informan que no pueden hacer nada en contra de las bocinas ruidosas cuando estas suenan dentro de los domicilios. Dicen que si te molesta, "pongas tu música también".
1. Dado lo anterior, solo quedaba una cosa por hacer. Poner mi música a todo volumen, siendo mi música una combinación de silencios a su máxima potencia.

Señoras y señores, damas y caballeros, me complace presentarles a la máquina generadora de silencio.

:::note[Nota]
Puedes leer todos los artículos relacionados a este tema en:

+ <a href="https://elcaza.github.io/archive/?tag=silence_machine" target="_blank">Contexto de la máquina generadora de silencio</a>
:::

# ¿Cómo funciona?
Puedes ver una explicación profunda en:
+ <a href="https://elcaza.github.io/posts/maker/canales_wifi_bt/" target="_blank">Canales WiFi y Bluetooth</a>

De manera resumida, imagina que para que tu bocina reproduzca música requiere enviar datos a través de una línea teléfonica distinta a una velocidad de 1600 veces por segundo. Este proyecto busca que esas líneas estén ocupadas transmitiendo silencio. Básicamente, como cuando pones dos bocinas a todo volumen y la música se mezcla entre sí.

# Componentes requeridos
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
	
# Modo de operación
1. Prende el dispositivo
1. Se corre de forma automática un chequeo de salud, se muestra el resultado en la pantalla
	+ Si todo sale bien, puedes proseguir 
	+ Si alguno de los módulos RF falla, el resultado se mostrara en pantalla
	+ Si la pantalla falla, en el Serial se mostrará el error
	+ En muchas ocasiones, solo con reiniciarlo, `botón #`, se corregirá el error
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

# Modos de silencio (Single-core)
+ silence_mode_1
+ silence_mode_2
+ silence_mode_3
+ silence_mode_4

# Tabla de conexiones

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

# Crea tus propios modos de silencio
Este proyecto es posible gracias a proyectos similares que eran Open Source, por lo que este proyecto también lo es y busca:
1. Ser fácil de leer, analizar y modificar
1. Permitir añadir modos de silencio personalizados
1. Ser lo más eficiente posible
	+ Durante el desarrollo de este proyecto se realizó un análisis y comparación de resultados, dando como resultado un proyecto más eficiente.
	+ En las próximas semanas publicaré el resultado de algunas comparativas. (Espera el link aquí)

## Composición del código
1. Librerías
1. Sección de configuraciones que puedes modificar
	+ Configuración de pantalla
	+ Configuración de radios
	+ Configuración de botones
	+ Configuraciones adicionales
1. Definición de variables
	+ Radios
	+ Botones
	+ Pantalla
1. Configuración de modos de silencio (Puedes añadir los tuyos)
	1. Array de descripciones para cada modo de silencio
	1. Funciones de los modos de silencio (La lógica del modo)
	1. Menú para los modos de silencio
1. Funciones de operación
	+ show_health_test
	+ show_nav
	+ show_current_mode
	+ press_to_start
	+ start_nav
1. Función setup que inicializa el código

## ¿Cómo añadir tus propios modos de silencio?
Puedes crear tus propios métodos para generar silencio. Para esto debes tomar en cuenta las siguientes consideraciones:
1. Frecuencias 2.4 Ghz
	+ Bluetooth Clásico (2.402 GHz y 2.480 GHz)
		+ 79 canales
		+ 1 MHz cada uno
		+ Salto de Frecuencia (FHSS) - 1600/s
	+ BLE (2.402 GHz y 2.480 GHz)
		+ 40 canales
		+ 2 MHz
		+ Salto de Frecuencia (FHSS) - 1600/s
		+ Advertising Channels - 37, 38 y 39
	+ Dicho lo anterior, para la librería RF24 tenemos los siguientes canales
		+ Frecuencia(MHz) = 2400 + Número de Canal
			+ CH 0 => 2400
			+ CH 1 => 2401
			+ ...
			+ CH 80 => 2480

## Creación de tus propios custom modes
Modo de creación (`custom_mode`)
1. Añades tu descripción de tu `custom_mode` al array `const char* all_modes[] = {};`
1. Creas una función que coincida con el identificador numérico de tu `custom_mode` en `void silence_mode_#() {}`
1. Añade el `case` correspondiente a tu `custom_mode` en `void generate_silence(int p_index) {}`

## Pautas para la integración de código
+ Identación por tabuladores de 4 espacios de longitud
+ Homologación de nombres
+ Que los modos tengan un propósito definido y útil

## Diagrama de funcionamiento

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/silence_machine/diagrama.png" width="100%">

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