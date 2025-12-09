---
title: Genera diccionarios con Crunch
published: 2025-12-08
description: 'Genera diccionarios con Crunch'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/crunch/portada.jpg'
tags: [Hacking, Wordlist]
category: 'Hacking'
draft: false 
lang: 'es'
---

# ¿Qué es Crunch?

Crunch es una herramienta para generar listas de palabras o diccionarios para realizar ataques de fuerza bruta.

# Primeros pasos

## Instalación

~~~bash
sudo apt install crunch
~~~

## Desinstalación

~~~bash
sudo apt remove crunch
~~~

## Ubicaciones por defecto

### Charset list
+ `/usr/share/crunch/charset.lst`

### Contenido de Charset list
Puedes añadir tu propio charset custom
+ `custom = [+-*/1abc.] `

~~~bash
# charset configuration file for winrtgen v1.2 by Massimiliano Montoro (mao@oxid.it)
# compatible with rainbowcrack 1.1 and later by Zhu Shuanglei <shuanglei@hotmail.com>


hex-lower                     = [0123456789abcdef]
hex-upper                     = [0123456789ABCDEF]

numeric                       = [0123456789]
numeric-space                 = [0123456789 ]

symbols14                     = [!@#$%^&*()-_+=]
symbols14-space               = [!@#$%^&*()-_+= ]

symbols-all                   = [!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
symbols-all-space             = [!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]

ualpha                        = [ABCDEFGHIJKLMNOPQRSTUVWXYZ]
ualpha-space                  = [ABCDEFGHIJKLMNOPQRSTUVWXYZ ]
ualpha-numeric                = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789]
ualpha-numeric-space          = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ]
ualpha-numeric-symbol14       = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=]
ualpha-numeric-symbol14-space = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+= ]
ualpha-numeric-all            = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
ualpha-numeric-all-space      = [ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]

lalpha                        = [abcdefghijklmnopqrstuvwxyz]
lalpha-space                  = [abcdefghijklmnopqrstuvwxyz ]
lalpha-numeric                = [abcdefghijklmnopqrstuvwxyz0123456789]
lalpha-numeric-space          = [abcdefghijklmnopqrstuvwxyz0123456789 ]
lalpha-numeric-symbol14       = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()-_+=]
lalpha-numeric-symbol14-space = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()-_+= ]
lalpha-numeric-all 	      = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
lalpha-numeric-all-space      = [abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]

mixalpha                   = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ]
mixalpha-space             = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ ]
mixalpha-numeric           = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789]
mixalpha-numeric-space     = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789 ]
mixalpha-numeric-symbol14  = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=]
mixalpha-numeric-symbol14-space = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+= ]
mixalpha-numeric-all       = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
mixalpha-numeric-all-space = [abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]

#########################################################################################
#                 SWEDISH CHAR-SUPPORT                                                  # #########################################################################################

#########################
# Uppercase             #
#########################
ualpha-sv                        = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ]
ualpha-space-sv                  = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ ]
ualpha-numeric-sv                = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789]
ualpha-numeric-space-sv          = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789 ]
ualpha-numeric-symbol14-sv       = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+=]
ualpha-numeric-symbol14-space-sv = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+= ]
ualpha-numeric-all-sv            = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
ualpha-numeric-all-space-sv      = [ABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]

#########################
# Lowercase             #
#########################
lalpha-sv                        = [abcdefghijklmnopqrstuvwxyzåäö]
lalpha-space-sv                  = [abcdefghijklmnopqrstuvwxyzåäö ]
lalpha-numeric-sv                = [abcdefghijklmnopqrstuvwxyzåäö0123456789]
lalpha-numeric-space-sv          = [abcdefghijklmnopqrstuvwxyzåäö0123456789 ]
lalpha-numeric-symbol14-sv       = [abcdefghijklmnopqrstuvwxyzåäö0123456789!@#$%^&*()-_+=]
lalpha-numeric-symbol14-space-sv = [abcdefghijklmnopqrstuvwxyzåäö0123456789!@#$%^&*()-_+= ]
lalpha-numeric-all-sv            = [abcdefghijklmnopqrstuvwxyzåäö0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
lalpha-numeric-all-space-sv      = [abcdefghijklmnopqrstuvwxyzåäö0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]

#########################
# Mixcase               #
#########################
mixalpha-sv                   = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ]
mixalpha-space-sv             = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ ]
mixalpha-numeric-sv           = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789]
mixalpha-numeric-space-sv     = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789 ]
mixalpha-numeric-symbol14-sv  = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+=]
mixalpha-numeric-symbol14-space-sv = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+= ]
mixalpha-numeric-all-sv       = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/]
mixalpha-numeric-all-space-sv = [abcdefghijklmnopqrstuvwxyzåäöABCDEFGHIJKLMNOPQRSTUVWXYZÅÄÖ0123456789!@#$%^&*()-_+=~`[]{}|\:;"'<>,.?/ ]
~~~

# ¿Cómo usarlo?

~~~bash
crunch <min> <max> [options]
~~~

## Opciones principales
+ -o wordlist.txt (output)
+ -f /path/to/charset.lst charset-name
+ -t @,%^ Specifies  a pattern
+ Hay más opciones que puedes consultar con `man crunch`

# Ejemplos

## Pin de 4 digitos

~~~bash
crunch 4 4 0123456789

# 0000
# 0001
# ...
# 9999
~~~

## Combinación binaria de 2 a 4 digitos

~~~bash
crunch 2 4 01

# 00
# 01
# ...
# 1110
# 1111
~~~

## Combinación binaria de 2 a 4 caracteres usando abc

~~~bash
crunch 2 4 abc

# aa
# bb
# ...
# cccb
# cccc
~~~

## Letras usuando el charset por defecto
~~~bash
crunch 1 1

# a
# b
# ...
# z
~~~

## Charsets incluidos
~~~bash
crunch 1 2 -f /usr/share/crunch/charset.lst mixalpha-numeric-all-space -o wordlist.txt

# a
# b
# ...
# /
#   <== Son dos espacios
~~~
+ La opción `-o` es para guardar el output como archivo

## Patrones
1. Se usa para generar iteraciones sobre un password conocido, por ejemplo si sabes que tu contraseña contiene la palabra "dog"

### Opciones
~~~
-t @,%^
              Specifies a pattern, eg: @@god@@@@ where the only the @'s, ,'s, %'s, and ^'s will change.
              @ will insert lower case characters
              , will insert upper case characters
              % will insert numbers
              ^ will insert symbols
~~~

### Ejemplo 1
Lo anterior funciona cuando se usa la siguiente sintaxis:

~~~bash
crunch 2 2 -t @%

# a0
# a1
# ...
# z8
# z9
~~~

### Ejemplo 2
Cuando usamos la opción `-f` se toma el charset que defines y el `@` es cualquier caracter

~~~bash
crunch 4 4 -f /usr/share/crunch/custom.lst custom -t dog@ 

# doga
# dogZ
# dog5
# ...
# dog=
# dog  <= Es un espacio
~~~

### Ejemplo 3
Si el patrón cuenta con caracteres especiales puedes usar las comillas simples `''` o dobles `""` para evitar conflictos

~~~bash
crunch 4 4 -f /usr/share/crunch/charset.lst mixalpha-numeric-all-space -t "do'%"
# do'0
# ...
# do'9

crunch 4 4 -f /usr/share/crunch/charset.lst mixalpha-numeric-all-space -t 'do"%'
# do"0
# ...
# do"9

crunch 4 4 -f /usr/share/crunch/charset.lst mixalpha-numeric-all-space -t "d'\"%"
# d'"0
# ...
# d'"9
~~~

### Ejemplo 4

~~~bash
crunch 4 5 -p dog cat bird
~~~
+ The numbers aren't processed but are needed.

### Ejemplo 5

~~~bash
crunch 3 3 ab + 01 '!@#' -t @%^
# a0!
# a0@
# a0#
# a1!
# a1@
# a1#
# b0!
# b0@
# b0#
# b1!
# b1@
# b1#
~~~

### Ejemplo 6

~~~bash
crunch 3 3 -t dd% -p dog cat
# catdog0
# catdog1
# catdog2
# catdog3
# catdog4
# catdog5
# catdog6
# catdog7
# catdog8
# catdog9
# dogcat0
# dogcat1
# dogcat2
# dogcat3
# dogcat4
# dogcat5
# dogcat6
# dogcat7
# dogcat8
# dogcat9
~~~

### Ejemplo 7
Cuando usamos la opción `-f` se toma el charset que defines y el `@` es cualquier caracter
Pero al usar la opción `-l` hace que el `@` sea tratao como literal

~~~bash
crunch 5 5 -t p@ss% -l a@aaa
# p@ss0
# p@ss1
# ...
# p@ss8
# p@ss9
~~~

# Más información
+ <a href="https://manpages.ubuntu.com/manpages/bionic/man1/crunch.1.html" target="_blank">Man page Ubuntu - Crunch</a>
+ <a href="https://www.kali.org/tools/crunch/" target="_blank">Kali crunch</a>


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