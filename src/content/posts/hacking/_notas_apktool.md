---
title: Decompilar y compilar aplicaciones móviles con apktool
published: 2025-11-13
description: 'Decompilar y compilar aplicaciones móviles con apktool'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/hacking/apktool/portada.jpg'
tags: [Hacking, Android]
category: 'Hacking'
draft: true 
lang: 'es'
---

# Info
Máquina Virtual Dalvik/ART (el entorno de ejecución de Android)

# ¿Qué se puede buscar?

~~~bash
su
magisk
busybox
test-keys
/system/bin/su
/sbin/su
~~~

# Estrategia de localización en Smali

La manera más eficiente y segura es enfocarse en el método que:
1. Es público: (se usa la directiva .method public)
1. No toma argumentos: (su firma es ()Z)
1. Retorna un valor booleano primitivo: (Z en Smali)

Aplicar el Parche (El cuerpo del método)
Para cualquier método que cumpla la firma `()Z` (o `(Z)Z` si tiene argumentos, pero devolviendo Z), reemplaza el cuerpo por el siguiente código. 
Este código ignora toda la lógica y fuerza un retorno de false (representado por 0).

~~~bash
.method public a()Z
    .locals 1    # Solo necesitamos un registro local (v0)

    const/4 v0, 0x0  # Carga el valor 0 (false) en el registro v0
    return v0        # Retorna el valor 0 (false)
.end method
~~~

## ¿Qué hace const/4 v0, 0x0?

<table>
  <thead>
    <tr>
      <th>Parte de la Instrucción</th>
      <th>Significado</th>
      <th>Función en el Parche</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>const/4</td>
      <td>
        Constante de 4 bits. Es la instrucción de Smali para cargar un valor entero
        pequeño (de -8 a 7) de forma muy eficiente.
      </td>
      <td>Indica que se va a cargar un valor constante.</td>
    </tr>
    <tr>
      <td>v0</td>
      <td>Registro Local v0</td>
      <td>Es una variable temporal (registro) donde se almacena el valor que se va a cargar.</td>
    </tr>
    <tr>
      <td>0x0</td>
      <td>Valor Cero ( 0 )</td>
      <td>Es el valor constante que se está cargando en el registro.</td>
    </tr>
  </tbody>
</table>


<table>
  <thead>
	<tr>
		<th>Descriptor Smali</th>
		<th>Tipo Java/Kotlin</th>
		<th>Nombre Completo</th>
		<th>¿Cómo se Parchea?</th>
	</tr>
  </thead>
  <tbody>
	<tr>
		<td>Z</td>
		<td>boolean</td>
		<td>Booleano Primitivo</td>
		<td>const/4 v0, 0x0 y return v0 (retorna false).</td>
	</tr>
	<tr>
		<td>B</td>
		<td>byte</td>
		<td>Byte Primitivo</td>
		<td>const/4 v0, 0x0 y return v0 (retorna 0).</td>
	</tr>
	<tr>
		<td>I</td>
		<td>int</td>
		<td>Entero Primitivo</td>
		<td>const/4 v0, 0x0 y return v0 (retorna 0).</td>
	</tr>
	<tr>
		<td>J</td>
		<td>long</td>
		<td>Entero Largo Primitivo</td>
		<td>const-wide/16 v0, 0x0 y return-wide v0 (retorna 0L).</td>
	</tr>
	<tr>
		<td>V</td>
		<td>void</td>
		<td>Sin Retorno</td>
		<td>return-void. (Si el cuerpo del método realiza acciones, simplemente lo vacías y dejas return-void).</td>
	</tr>
	<tr>
		<td>L</td>
		<td>Clases/Objetos</td>
		<td>Tipo de Referencia</td>
		<td>const/4 v0, 0x0 y return-object v0 (retorna null). Opcionalmente, se debe retornar un objeto empacado.</td>
	</tr>
	<tr>
		<td>[</td>
		<td>Arrays</td>
		<td>Tipo de Array</td>
		<td>const/4 v0, 0x0 y return-object v0 (retorna null).</td>
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