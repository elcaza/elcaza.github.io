---
title: Diagramas de los módulos de radiofrecuencia 2.4
published: 2025-10-24
description: 'Los módulos nRF24L01 PA LNA y variantes'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/rf_diagramas/nrf24l01_pa_lna.png'
tags: [Maker, silence_machine]
category: 'Maker'
draft: false 
lang: 'es'
---

# ¿Qué es el módulo nRF24L01 PA LNA?
+ Es un transceptor inalámbrico de radiofrecuencia (RF) 
+ Opera en la banda de 2.4 GHz
+ Diseñado para la comunicación de datos a media y larga distancia en proyectos de electrónica y microcontroladores

## Composición del nombre
+ nRF24L01 => Nombre
+ PA => Power Amplifier (Amplificador de Potencia)
+ LNA => Low-Noise Amplifier (Amplificador de Bajo Ruido)

## Características y especificaciones
+ Frecuencia de operación: Banda ISM de 2.4 GHz
+ Alcance: Típicamente hasta 1 km en línea de vista (a 250 Kbps)
+ Velocidad de datos: Seleccionable entre 250 Kbps, 1 Mbps y 2 Mbps
	+ A mayor velocidad de datos, menor es el alcance efectivo
+ Interfaz: Utiliza el bus SPI (Serial Peripheral Interface) para la comunicación con microcontroladores
+ Antena: Suele incluir un conector SMA y una antena externa (a diferencia de la antena PCB integrada del modelo estándar)
+ Voltaje de alimentación: Funciona a 3.3V 
	+ **Es importante. Si se conecta a 5v se quema.**

# Diagrama nRF24L01 PA LNA
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/rf_diagramas/nrf24l01_pa_lna.png" width="100%">

# ¿Qué hace cada PIN?
<table>
	<thead>
		<tr>
			<th>PIN Number</th>
			<th>PIN Name</th>
			<th>Abbreviation</th>
			<th>Function</th>
			<th>Comentarios</th>
			<th>Color de cable</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>GND</td>
			<td>Ground</td>
			<td>Connected to the Ground of the system</td>
			<td>Conexión a tierra</td>
			<td>Negro ⬛</td>
		</tr>
		<tr>
			<td>2</td>
			<td>VCC</td>
			<td>Power</td>
			<td>Powers the module using 3.3V</td>
			<td>Alimentación 3.3V</td>
			<td>Rojo 🔴</td>
		</tr>
		<tr>
			<td>3</td>
			<td>CE</td>
			<td>Chip Enable</td>
			<td>Used to enable SPI communication (Serial Peripheral Interface)</td>
			<td>Usado principalmente para la transferencia de información entre circuitos integrados en equipos electrónicos</td>
			<td>Amarillo 🟡</td>
		</tr>
		<tr>
			<td>4</td>
			<td>CSN</td>
			<td>Chip Select Not</td>
			<td>This pin has to be kept high always, else it will disable the SPI</td>
			<td></td>
			<td>Naranja 🟠</td>
		</tr>
		<tr>
			<td>5</td>
			<td>SCK</td>
			<td>Serial Clock</td>
			<td>Provides the clock pulse using which the SPI communications works</td>
			<td>Provee la frecuencia de reloj</td>
			<td>Verde 🟢</td>
		</tr>
		<tr>
			<td>6</td>
			<td>MOSI</td>
			<td>Master Out Slave In</td>
			<td>Connected to MOSI pin of MCU, for the module to receive data from MCU</td>
			<td>Recibir información</td>
			<td>Azul 🔵</td>
		</tr>
		<tr>
			<td>7</td>
			<td>MISO</td>
			<td>Master In Slave Out</td>
			<td>Connected to MISO pin of MCU, for the module to send data from the MCU</td>
			<td>Enviar información</td>
			<td>Morado 🟣</td>
		</tr>
		<tr>
			<td>8</td>
			<td>IRQ</td>
			<td>Interrupt</td>
			<td>It is an active low pin and is used only if interrupt is required</td>
			<td>PIN de interrupción</td>
			<td></td>
		</tr>
	</tbody>
</table>

# Comparativa con otros módulos
A menudo se hace la comparativa con otros módulos de radio. Algunos de ellos son:
+ E01-2G4M27D
+ E01-ML01DP5
+ NRF24l01+PA+LNA

El mapa de conexión es el mismo. Una cosa menos de la que preocuparse. Respecto a la comparativa teórica tenemos la siguiente tabla:

<table>
	<thead>
		<tr>
			<th>Característica</th>
			<th>nRF24L01+PA+LNA (Genérico)</th>
			<th>E01-ML01DP5 (Ebyte)</th>
			<th>E01-2G4M27D (Ebyte)</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Chip base</td>
			<td>nRF24L01P / nRF24L01+</td>
			<td>nRF24L01P / nRF24L01+</td>
			<td>nRF24L01P / nRF24L01+</td>
		</tr>
		<tr>
			<td>Potencia TX máxima (PA)</td>
			<td>+20 dBm (100 mW)</td>
			<td>+20 dBm (100 mW)</td>
			<td>+27 dBm (500 mW)</td>
		</tr>
		<tr>
			<td>Sensibilidad RX máxima (LNA)</td>
			<td>≈ -104 dBm @ 250 Kbps</td>
			<td>≈ -106 dBm @ 250 Kbps</td>
			<td>≈ -99 dBm @ 250 Kbps</td>
		</tr>
		<tr>
			<td>Alcance máximo (teórico)</td>
			<td>≈ 1,000 m</td>
			<td>≈ 2,100 - 2,500 m</td>
			<td>≈ 2,500 - 5,000 m</td>
		</tr>
		<tr>
			<td>Corriente TX (Máx.)</td>
			<td>≈ 115 mA</td>
			<td>≈ 130 mA</td>
			<td>≈ 380 mA</td>
		</tr>
		<tr>
			<td>Antena</td>
			<td>Conector SMA-K (Externa)</td>
			<td>Conector SMA-K (Externa)</td>
			<td>Conector SMA-K (Externa)</td>
		</tr>
		<tr>
			<td>Tasa de datos</td>
			<td>250 Kbps, 1 Mbps, 2 Mbps</td>
			<td>250 Kbps, 1 Mbps, 2 Mbps</td>
			<td>250 Kbps, 1 Mbps, 2 Mbps</td>
		</tr>
		<tr>
			<td>Voltaje de alimentación</td>
			<td>3v</td>
			<td>3v</td>
			<td>5v</td>
		</tr>
	</tbody>
</table>

# Información adicional que podría ser de tu interés
+ <a href="https://www.youtube.com/watch?v=BmkBTmLXF9s" target="_blank">Distancia Máxima con NRF24l01</a>
+ <a href="https://www.youtube.com/watch?v=cc5cD8xBYdA" target="_blank">NRF24L01+ / NRF24L01+PA+LNA / E01-ML01DP5 / E01-2G4M27D -Transceiver test 250 Kbps, 1 Mbps, 2 Mbps</a>

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