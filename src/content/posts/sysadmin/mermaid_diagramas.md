---
title: Crea diagramas al estilo Markdown con Mermaid
published: 2025-10-27
description: 'Mermaid es una manera sencilla de crear múltiples tipos de diagramas. Una joya para los que, como yo, no le saben a las artes del diseño'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/mermaid_diagramas/portada.png'
tags: [Sysadmin]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# ¿Qué es Mermaid?
Mermaid es una herramienta de código abierto basada en JavaScript que permite crear diagramas y visualizaciones a partir de descripciones de texto, de manera similar a Markdown. 

Se utiliza para generar dinámicamente diagramas como diagramas de flujo, UML, organigramas y otros tipos de gráficos, lo que simplifica su creación y mantenimiento. 


# Herramienta
+ <a href="https://mermaid.live/" target="_blank">Mermaid</a>

# Recomendaciones y ejemplo de sintaxis
1. Define primero todas tus variables
2. Une los puntos
~~~mermaid
flowchart TD
    A[Inicio] 
    B[Test de salud]
    C{Opciones}
    E[Continuar]
    G[Reiniciar]
    H{Modo}
    I[Generar silencio]
    J[Reiniciar]

    A --> B --> C
    C -->|Botón 3 - #| G --> A
    C -->|Botón 4 - *| E --> H
    H -->|Botón 4 - *| I
    H -->|Botón 3 - #| J --> A
~~~

# ¿Qué puedes lograr?

Puedes ver todos sus ejemplos en su <a href="https://mermaid.live/" target="_blank">sitio web</a>, algunos de mis favoritos son los siguientes:

## Diagramas
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/mermaid_diagramas/portada.png" width="100%">

## Líneas de tiempo
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/mermaid_diagramas/timeline.png" width="100%">

## Mapas mentales
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/mermaid_diagramas/mapa_mental.png" width="100%">

## Diagramas de Gantt
<img src="https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/mermaid_diagramas/gantt.png" width="100%">

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