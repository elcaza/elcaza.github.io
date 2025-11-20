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

<table>
    <thead>
        <tr>
            <th>Canal (k)</th>
            <th>Frecuencia Central (MHz)</th>
            <th>Canal (k)</th>
            <th>Frecuencia Central (MHz)</th>
            <th>Canal (k)</th>
            <th>Frecuencia Central (MHz)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0</td>
            <td>2402</td>
            <td>27</td>
            <td>2429</td>
            <td>54</td>
            <td>2456</td>
        </tr>
        <tr>
            <td>1</td>
            <td>2403</td>
            <td>28</td>
            <td>2430</td>
            <td>55</td>
            <td>2457</td>
        </tr>
        <tr>
            <td>2</td>
            <td>2404</td>
            <td>29</td>
            <td>2431</td>
            <td>56</td>
            <td>2458</td>
        </tr>
        <tr>
            <td>3</td>
            <td>2405</td>
            <td>30</td>
            <td>2432</td>
            <td>57</td>
            <td>2459</td>
        </tr>
        <tr>
            <td>4</td>
            <td>2406</td>
            <td>31</td>
            <td>2433</td>
            <td>58</td>
            <td>2460</td>
        </tr>
        <tr>
            <td>5</td>
            <td>2407</td>
            <td>32</td>
            <td>2434</td>
            <td>59</td>
            <td>2461</td>
        </tr>
        <tr>
            <td>6</td>
            <td>2408</td>
            <td>33</td>
            <td>2435</td>
            <td>60</td>
            <td>2462</td>
        </tr>
        <tr>
            <td>7</td>
            <td>2409</td>
            <td>34</td>
            <td>2436</td>
            <td>61</td>
            <td>2463</td>
        </tr>
        <tr>
            <td>8</td>
            <td>2410</td>
            <td>35</td>
            <td>2437</td>
            <td>62</td>
            <td>2464</td>
        </tr>
        <tr>
            <td>9</td>
            <td>2411</td>
            <td>36</td>
            <td>2438</td>
            <td>63</td>
            <td>2465</td>
        </tr>
        <tr>
            <td>10</td>
            <td>2412</td>
            <td>37</td>
            <td>2439</td>
            <td>64</td>
            <td>2466</td>
        </tr>
        <tr>
            <td>11</td>
            <td>2413</td>
            <td>38</td>
            <td>2440</td>
            <td>65</td>
            <td>2467</td>
        </tr>
        <tr>
            <td>12</td>
            <td>2414</td>
            <td>39</td>
            <td>2441</td>
            <td>66</td>
            <td>2468</td>
        </tr>
        <tr>
            <td>13</td>
            <td>2415</td>
            <td>40</td>
            <td>2442</td>
            <td>67</td>
            <td>2469</td>
        </tr>
        <tr>
            <td>14</td>
            <td>2416</td>
            <td>41</td>
            <td>2443</td>
            <td>68</td>
            <td>2470</td>
        </tr>
        <tr>
            <td>15</td>
            <td>2417</td>
            <td>42</td>
            <td>2444</td>
            <td>69</td>
            <td>2471</td>
        </tr>
        <tr>
            <td>16</td>
            <td>2418</td>
            <td>43</td>
            <td>2445</td>
            <td>70</td>
            <td>2472</td>
        </tr>
        <tr>
            <td>17</td>
            <td>2419</td>
            <td>44</td>
            <td>2446</td>
            <td>71</td>
            <td>2473</td>
        </tr>
        <tr>
            <td>18</td>
            <td>2420</td>
            <td>45</td>
            <td>2447</td>
            <td>72</td>
            <td>2474</td>
        </tr>
        <tr>
            <td>19</td>
            <td>2421</td>
            <td>46</td>
            <td>2448</td>
            <td>73</td>
            <td>2475</td>
        </tr>
        <tr>
            <td>20</td>
            <td>2422</td>
            <td>47</td>
            <td>2449</td>
            <td>74</td>
            <td>2476</td>
        </tr>
        <tr>
            <td>21</td>
            <td>2423</td>
            <td>48</td>
            <td>2450</td>
            <td>75</td>
            <td>2477</td>
        </tr>
        <tr>
            <td>22</td>
            <td>2424</td>
            <td>49</td>
            <td>2451</td>
            <td>76</td>
            <td>2478</td>
        </tr>
        <tr>
            <td>23</td>
            <td>2425</td>
            <td>50</td>
            <td>2452</td>
            <td>77</td>
            <td>2479</td>
        </tr>
        <tr>
            <td>24</td>
            <td>2426</td>
            <td>51</td>
            <td>2453</td>
            <td>78</td>
            <td>2480</td>
        </tr>
        <tr>
            <td>25</td>
            <td>2427</td>
            <td>52</td>
            <td>2454</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>26</td>
            <td>2428</td>
            <td>53</td>
            <td>2455</td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

## Banda 2.4ghz - BLE (2.402 GHz y 2.480 GHz)​
Canales
+ Divide esta banda en 40 canales individuales ​
+ Ancho 2Mhz​
+ Los canales están numerados del 0 al 39

Salto de Frecuencia (FHSS)​
+ 79 canales de forma pseudoaleatoria, hasta 1600 veces por segundo​
+ 3 Advertising Channels​: 37, 38 y 39

<table>
	<thead>
		<tr>
			<th>Canal (No.)</th>
			<th>Frecuencia Central (MHz)</th>
			<th>Rango de Frecuencia (MHz)</th>
			<th>Clasificación</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><strong>37</strong></td>
			<td><strong>2402</strong></td>
			<td>2401 - 2403</td>
			<td><strong>Advertising (Publicidad)</strong></td>
		</tr>
		<tr>
			<td>0</td>
			<td>2404</td>
			<td>2403 - 2405</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>1</td>
			<td>2406</td>
			<td>2405 - 2407</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>2</td>
			<td>2408</td>
			<td>2407 - 2409</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>3</td>
			<td>2410</td>
			<td>2409 - 2411</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>4</td>
			<td>2412</td>
			<td>2411 - 2413</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>5</td>
			<td>2414</td>
			<td>2413 - 2415</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>6</td>
			<td>2416</td>
			<td>2415 - 2417</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>7</td>
			<td>2418</td>
			<td>2417 - 2419</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>8</td>
			<td>2420</td>
			<td>2419 - 2421</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>9</td>
			<td>2422</td>
			<td>2421 - 2423</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>10</td>
			<td>2424</td>
			<td>2423 - 2425</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td><strong>38</strong></td>
			<td><strong>2426</strong></td>
			<td>2425 - 2427</td>
			<td><strong>Advertising (Publicidad)</strong></td>
		</tr>
		<tr>
			<td>11</td>
			<td>2428</td>
			<td>2427 - 2429</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>12</td>
			<td>2430</td>
			<td>2429 - 2431</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>13</td>
			<td>2432</td>
			<td>2431 - 2433</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>14</td>
			<td>2434</td>
			<td>2433 - 2435</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>15</td>
			<td>2436</td>
			<td>2435 - 2437</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>16</td>
			<td>2438</td>
			<td>2437 - 2439</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>17</td>
			<td>2440</td>
			<td>2439 - 2441</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>18</td>
			<td>2442</td>
			<td>2441 - 2443</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>19</td>
			<td>2444</td>
			<td>2443 - 2445</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>20</td>
			<td>2446</td>
			<td>2445 - 2447</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>21</td>
			<td>2448</td>
			<td>2447 - 2449</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>22</td>
			<td>2450</td>
			<td>2449 - 2451</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>23</td>
			<td>2452</td>
			<td>2451 - 2453</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>24</td>
			<td>2454</td>
			<td>2453 - 2455</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>25</td>
			<td>2456</td>
			<td>2455 - 2457</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>26</td>
			<td>2458</td>
			<td>2457 - 2459</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>27</td>
			<td>2460</td>
			<td>2459 - 2461</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>28</td>
			<td>2462</td>
			<td>2461 - 2463</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>29</td>
			<td>2464</td>
			<td>2463 - 2465</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>30</td>
			<td>2466</td>
			<td>2465 - 2467</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>31</td>
			<td>2468</td>
			<td>2467 - 2469</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>32</td>
			<td>2470</td>
			<td>2469 - 2471</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>33</td>
			<td>2472</td>
			<td>2471 - 2473</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>34</td>
			<td>2474</td>
			<td>2473 - 2475</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>35</td>
			<td>2476</td>
			<td>2475 - 2477</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td>36</td>
			<td>2478</td>
			<td>2477 - 2479</td>
			<td>Datos</td>
		</tr>
		<tr>
			<td><strong>39</strong></td>
			<td><strong>2480</strong></td>
			<td>2479 - 2481</td>
			<td><strong>Advertising (Publicidad)</strong></td>
		</tr>
	</tbody>
</table>

# Solapamiento de canales
## WIFI ​
+ 2.412 GHz a 2.484 GHz​
+ 14 canales​
+ 20 MHz​

## Bluetooth Clásico (BR/EDR) Basic Rate/Enhanced Data Rate o BR/EDR​
+ 2.402 GHz y 2.480 GHz​
+ 79 canales​
+ 1 MHz​
+ Salto de Frecuencia (FHSS) - 1600/s​

## Bluetooth LE o BLE​
+ 2.402 GHz y 2.480 GHz​
+ 40 canales​
+ 2 MHz​
+ Salto de Frecuencia (FHSS) - 1600/s​
+ Advertising Channels
	+ 37 => 2402 => 2
	+ 38 => 2426 => 26
	+ 39​ => 2480 => 80

## Limites ideales:
+ Inferior 2
+ Superior 79

## Advertising
+ 37 => 2402 => 2
	+ 38 => 2426 => 26
	+ 39​ => 2480 => 80

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