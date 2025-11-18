---
title: Canales WiFi y Bluetooth
published: 2025-02-23
description: 'Los módulos nRF24L01 PA LNA y variantes'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/canales_wifi_bt/portada.png'
tags: [Maker, silence_machine]
category: 'Maker'
draft: false 
lang: 'es'
---

# Canales WiFi y Bluetooth
Los canales de Wi-Fi y Bluetooth son los rangos de frecuencia que utilizan estas tecnologías para transmitir datos en la banda de 2.4 GHz (una de las bandas ISM). Aunque comparten el mismo espectro, su forma de gestionarlo es diferente para evitar interferencia.

## Banda 2.4ghz - WIFI (2.412 GHz a 2.484 GHz)​
+ Cada canal tiene un ancho fijo
+ El ancho de banda nominal (principal) del canal es de 20 MHz
+ Todo se basa en la frecuencia central ±10
+ Se da 1 mhz de tolerancia para temas de ruido
+ Los canales sin solapamiento son 1,6 y 11
+ Hay 14 canales en total, cada uno de 20 MHz de ancho. En Norteamérica los canales permitidos son del 1 al 11.

<table>
	<thead>
		<tr>
			<th>Canal</th>
			<th>Frecuencia Central (MHz)</th>
			<th>Inicio de Frecuencia Aproximado (MHz)</th>
			<th>Final de Frecuencia Aproximado (MHz)</th>
			<th>Ancho (MHz)</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>2412</td>
			<td>2401</td>
			<td>2423</td>
			<td>22</td>
		</tr>
		<tr>
			<td>2</td>
			<td>2417</td>
			<td>2406</td>
			<td>2428</td>
			<td>22</td>
		</tr>
		<tr>
			<td>3</td>
			<td>2422</td>
			<td>2411</td>
			<td>2433</td>
			<td>22</td>
		</tr>
		<tr>
			<td>4</td>
			<td>2427</td>
			<td>2416</td>
			<td>2438</td>
			<td>22</td>
		</tr>
		<tr>
			<td>5</td>
			<td>2432</td>
			<td>2421</td>
			<td>2443</td>
			<td>22</td>
		</tr>
		<tr>
			<td>6</td>
			<td>2437</td>
			<td>2426</td>
			<td>2448</td>
			<td>22</td>
		</tr>
		<tr>
			<td>7</td>
			<td>2442</td>
			<td>2431</td>
			<td>2453</td>
			<td>22</td>
		</tr>
		<tr>
			<td>8</td>
			<td>2447</td>
			<td>2436</td>
			<td>2458</td>
			<td>22</td>
		</tr>
		<tr>
			<td>9</td>
			<td>2452</td>
			<td>2441</td>
			<td>2463</td>
			<td>22</td>
		</tr>
		<tr>
			<td>10</td>
			<td>2457</td>
			<td>2446</td>
			<td>2468</td>
			<td>22</td>
		</tr>
		<tr>
			<td>11</td>
			<td>2462</td>
			<td>2451</td>
			<td>2473</td>
			<td>22</td>
		</tr>
	</tbody>
</table>

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/canales_wifi_bt/1_wifi_canales.png" width="100%">

## Banda 2.4ghz - Bluetooth Clásico (2.402 GHz y 2.480 GHz)​
+ Divide esta banda en 79 canales individuales
+ Ancho de 1 MHz ​
+ Los canales están numerados del 0 al 78
+ Salto de Frecuencia (FHSS)​
	+ 79 canales de forma pseudoaleatoria, hasta 1600 veces por segundo

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/canales_wifi_bt/2_bt.jpeg" width="100%">

## Banda 2.4ghz - BLE (2.402 GHz y 2.480 GHz)​
Canales
+ Divide esta banda en 40 canales individuales ​
+ Ancho 2Mhz​
+ Los canales están numerados del 0 al 39

Salto de Frecuencia (FHSS)​
+ 79 canales de forma pseudoaleatoria, hasta 1600 veces por segundo​
+ 3 Advertising Channels​: 37, 38 y 39

# Solapamiento de canales
WIFI ​
+ 2.412 GHz a 2.484 GHz​
+ 14 canales​
+ 20 MHz​

Bluetooth Clásico (BR/EDR) Basic Rate/Enhanced Data Rate o BR/EDR​
+ 2.402 GHz y 2.480 GHz​
+ 79 canales​
+ 1 MHz​
+ Salto de Frecuencia (FHSS) - 1600/s​

Bluetooth LE o BLE​
+ 2.402 GHz y 2.480 GHz​
+ 40 canales​
+ 2 MHz​
+ Salto de Frecuencia (FHSS) - 1600/s​
+ Advertising Channels - 37, 38 y 39​

<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/canales_wifi_bt/4_all.png" width="100%">


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