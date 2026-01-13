---
title: Evita desconexiones abruptas en una raspberry pi y SSD vía usb 3.0
published: 2026-01-07
description: 'La raspberry pi expulsa de manera abrupta los SSDs conectados a través de USB 3.0.'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/ssd_raspberry/portada.jpg'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# El problema
La raspberry pi expulsa de manera abrupta los SSDs conectados a través de USB 3.0. Aparentemente es por los picos de energía que consume el protocolo UAS (USB Attached SCSI).

# La solución
Forzar a la Raspberry Pi a usar un protocolo más antiguo pero muy estable llamado Bulk-Only Transport (BOT).

## Procedimiento

### 1. Identificar el ID del dispositivo
~~~bash
lsusb
~~~
+ Bus 002 Device 003: ID `0a0a:1234` Realtek Semiconductor Corp. Nombre del dispositivo
+ Copia el **ID**: `0a0a:1234`

### 2. Añadir la excepción de UAS para el dispositivo
~~~bash
sudo vim /boot/firmware/cmdline.txt
~~~

Añade la siguiente línea con tu propio **ID**
~~~bash
usb-storage.quirks=0a0a:1234:u
~~~
+ Añadir al inicio o al final de archivo separado por un espacio. No añadir saltos de línea.

Si tienes dos o más discos los puedes añadir de la siguiente manera
~~~bash
usb-storage.quirks=0a0a:1234:u,0b0b:5678:u
~~~

### 3. Reinicia el equipo
~~~bash
sudo reboot
~~~

### 4. Verificar
~~~bash
dmesg | grep usb-storage
~~~
+ Debes observar: usb-storage 2-2:1.0: USB Mass Storage device detected usb-storage 2-2:1.0: Quirks match for vid abab pid 9200: 900000 (El número puede variar, pero debe decir Quirks match).

### 5. Tabla comparativa de velocidad/estabilidad

<table>
  <thead>
    <tr>
      <th>Configuración</th>
      <th>Velocidad Estimada</th>
      <th>Estado de Estabilidad</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>USB 3.0 Nativo (UASP)</td>
      <td>320 - 380 MB/s</td>
      <td>Inestable (Se desconecta)</td>
    </tr>
    <tr>
      <td>USB 3.0 con Quirk (Tu estado actual)</td>
      <td>210 - 260 MB/s</td>
      <td>Estable (Perfecto para servidores)</td>
    </tr>
    <tr>
      <td>USB 2.0 (Puerto negro)</td>
      <td>35 - 40 MB/s</td>
      <td>Estable (Pero muy lento)</td>
    </tr>
    <tr>
      <td>Tarjeta MicroSD Típica</td>
      <td>20 - 45 MB/s</td>
      <td>Estable (Pero vida útil corta)</td>
    </tr>
  </tbody>
</table>

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