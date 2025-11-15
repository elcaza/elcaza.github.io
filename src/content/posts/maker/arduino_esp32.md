---
title: Arduino IDE para ESP32
published: 2025-03-17
description: '¿Cómo instalar Arduino IDE para trabajar con el ESP32 en Linux?'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/maker/arduino_esp32/portada.png'
tags: [Maker, silence_machine]
category: 'Maker'
draft: false 
lang: 'es'
---

# ¿Cómo instalar Arduino IDE para trabajar con el ESP32 en Linux?

## Sistema utilizado

+ Debian 12 
+ Kernel 6.1.0-31-amd64

## Instalación de Arduino IDE

1. Descargamos Arduino IDE desde el sitio oficial (Linux - AppImage 64 bits (X86-64))
    + https://www.arduino.cc/en/software
1. Le damos permisos de ejecución al archivo descargado
    + `chmod +x arduino-ide_2.3.4_Linux_64bit.AppImage`
1. Creamos carpeta en /opt
    + `sudo mkdir /opt/arduino/`
1. Movemos el ejecutable de Arduino y asignamos un nuevo nombre
    + `sudo mv arduino-ide_2.3.4_Linux_64bit.AppImage /opt/arduino/arduino`
1. Para ejecutarlo se puede simplemente correr
    + `/opt/arduino/arduino`
1. De manera opcional se puede añadir un icono para la aplicación:
    + Crear un nuevo archivo en: `vim ~/.local/share/applications/arduino.desktop`

~~~bash
[Desktop Entry]
Version=1.0
Name=Arduino
Comment=Arduino IDE
Type=Application
Icon=/opt/arduino/arduino_logo.png
Exec=/opt/arduino/arduino
Terminal=false
Categories=Development
~~~

# ESP32 en Linux

1. Ir a `File > Preferences > additional boards` y añadir: 
    + `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
1. Instalar la biblioteca ESp2sota 
    + ESp2sota
1. Conectar la ESP32
1. Seleccionar la Board `DOIT ESP32 DEVKIT V1`
    + No vienen por orden alfabetico, se puede encontrar abajo del bloque "Adafruit Feather" y arriba del bloque "OLIMEX"
1. Seleccionas el Port correspondiente (variará acorde al puerto conectado)
1. Carga un código de prueba
    + Si llegase a mostrar el siguiente error `Permission denied: '/dev/ttyUSB0'` corre lo siguientes comandos

~~~bash
# Root only
ls -l /dev/ttyUSB0 
crw-rw---- 1 root dialout 188, 0 Feb 19 18:18 /dev/ttyUSB0

# chmod (selecciona el tty donde se encuentra conectada tu placa)
sudo chmod a+rw /dev/ttyUSB0

# Anyone have permissions to write
ls -l /dev/ttyUSB0 
crw-rw-rw- 1 root dialout 188, 0 Feb 19 18:18 /dev/ttyUSB0
~~~

## Posibles problemas

1. ¿No se reconoce mi ESP32?
    + Verifica que el cable que estás usando para la conexión transmita datos
    + Verifica los permisos del puerto usb
    + Podrían ser los drivers de tu ESP32

## Resumen

+ Instalar drivers (en caso de ser necesario)
+ Instalar Board
+ Instalar Bibliotecas

# esptool.py
Serial utility for flashing, provisioning, and interacting with Espressif SoCs

## Instalación en Linux

### Opción pipx 
+ Particularmente, la que prefiero
~~~bash
# Instalar pipx
sudo apt install pipx

# Instalar frida-tools <herramienta>
pipx install esptool

# Solamente si no lo detecta el PATH
pipx ensurepath
	# Note: '/home/user/.local/bin' is not on your PATH environment variable. These apps will not be globally accessible until your PATH is updated. Run `pipx ensurepath` to automatically add it, or manually
~~~

### Opción pyenv
~~~bash
# Virtual Environment Installation
python -m venv esptoolenv

# Activate Virtual Environment
source esptoolenv/bin/activate

# Instalar 
pip install esptool

# deactivate Virtual Environment
deactivate
~~~

## Comandos útiles

### Obtener información de tu ESP32

~~~bash
esptool.py flash_id

# sptool.py v4.9.0
# Found 2 serial ports
# Serial port /dev/ttyUSB0
# Connecting....
# Detecting chip type... Unsupported detection protocol, switching and trying again...
# Connecting....
# Detecting chip type... ESP32
# Chip is ESP32-D0WD-V3 (revision v3.1)
# Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
# Crystal is 40MHz
# MAC: 14:20:2f:fd:22:34
# Uploading stub...
# Running stub...
# Stub running...
# Manufacturer: 85
# Device: 2016
# Detected flash size: 4MB
# Flash voltage set by a strapping pin to 3.3V
# Hard resetting via RTS pin...
~~~

### Respaldar firmware de ESP32
+ Los parametros pueden cambiar dependiendo del `Detected flash size: 4MB`

~~~bash
# Save 1MByte or 8Mbit Flash
esptool.py --baud 115200 --port /dev/ttyUSB0 read_flash 0x0 0x100000 fw-backup-1M.bin 

# Save 4MByte or 32Mbit Flash
esptool.py --baud 115200 --port /dev/ttyUSB0 read_flash 0x0 0x400000 fw-backup-4M.bin

# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Restore 1MByte or 8Mbit Flash
esptool.py --baud 115200 --port /dev/ttyUSB0 write_flash 0x0 fw-backup-1M.bin 

# Restore 4MByte or 32Mbit Flash
esptool.py --baud 115200 --port /dev/ttyUSB0 write_flash 0x0 fw-backup-4M.bin

~~~

# Monitorear Arduino/ESP32/otros a través del puerto Serial
## Screen

~~~bash
# Instalar screen
sudo apt install screen

# Iniciarlo
screen /dev/ttyUSB0 115200

# Iniciarlo y guardar los logs
screen -L -Logfile archivo.log /dev/ttyUSB0 115200

# Cerrar el programa presionar
CTRL+A
\
# YES
~~~

Otra forma de guardar
+ You can also use `Ctrl + A`, `H` to save loggings into a screenlog.n file.
+ And one more `Ctrl + A`, `H` to turn it off.
+ `Ctrl + A, H`: Begins/ends logging of the current window to the file "screenlog.n".

# Conectar un botón con ezButton
## Código
~~~cpp
#include <ezButton.h>
ezButton button(5); // Defining GPIO5

void setup() {
  Serial.begin(9600);
  button.setDebounceTime(100); // It setting debounce time
  pinMode(5, INPUT_PULLUP); // It enables an internal pull-up resistor
  Serial.println("Listo");
}

void loop() {
  button.loop();
  if (button.isPressed()) {
    Serial.println("Boton presionado ====================");
  }
}
~~~

## Diagrama
+ Botón => GND
+ Botón => GPIO5

# Links 

## Drivers ESP32
+ https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads

## Error: Permission denied: '/dev/ttyUSB0'
+ Fuente: https://askubuntu.com/questions/1404787/permission-denied-dev-ttyusb0

## En caso de que te haga falta alguna libreria al momento de compilar
+ https://www.arduinolibraries.info/libraries/ez-button

## esptool.py
+ https://github.com/espressif/esptool
+ https://docs.espressif.com/projects/esptool/en/latest/esp32/installation.html

## Save and restore firmware of ESP32
+ https://cyberblogspot.com/how-to-save-and-restore-esp8266-and-esp32-firmware/

## Save output screen command 
+ https://stackoverflow.com/questions/14208001/save-screen-program-output-to-a-file

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