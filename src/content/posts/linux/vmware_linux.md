---
title: VMware Workstation en Debian 12 (2026)
published: 2026-01-23
description: 'Solucionan los múltiples problemas de usar VMware en HOST Linux con GUEST Linux en 2026'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/linux/vmware_linux/portada.jpg'
tags: [Linux]
category: 'Linux'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://drive.google.com/drive/folders/1oqjYrJ9QQF33mobSKb2sFO3Vp2lDOH4E?usp=sharing" target="_blank">VMware-Workstation-Full-17.5.2-23775571.x86_64.bundle</a>
    + La última versión "estable"
# El problema 

Las nuevas versiones de VMware funcionan fatal en Linux.
+ VMware-Workstation-Full-25H2-24995812.x86_64
+ VMware-Workstation-Full-17.6.4-24832109.x86_64

Cuentan con problemas como:
+ Lentitud
+ Mal funcionamiento de las vm-tools
    + Escalado de pantalla 
    + Clipboard
+ Atajos como `CTRL+SHIFT+ALT+RIGTH|LEFT` y `CTRL+IFT+ALT+RIGTH|LEFT` no funcionan
+ Las máquinas mueren si la pantalla se bloquea o suspende

# Las soluciones

Durante mi investigación encontré las siguientes soluciones
1. Estás usando Wayland
    + Debes verificar que en el GUEST y en el HOST uses XORG
    + En la pantalla de inicio de sesión hay una tuerquita o icono junto al password, ahí te da las opciones de inicio de sesión con XORG
1. Desde que los compró Broadcom todo empeoró
    + La última versión "estable" fue `VMware-Workstation-Full-17.5.2-23775571.x86_64.bundle`
    + Puedes regresar a ella o probar estas soluciones en tu versión actual
1. Si la `VM GUEST` se suspende o bloquea pierde comunicación con la máquina `HOST`
    + Quita las opciones de suspensión
1. Los atajos son interceptados por vmware (HOTKEYS)
    + Debes cambiar los atajos de vmware `Edit => Preferences => Hotkeys => COMBINACION`
    + A mi me vino bien `CTRL+SUPER` es decir `CTRL+WINDOWS`

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