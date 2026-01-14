---
title: n8n en Docker
published: 2025-12-21
description: 'Ejecuta n8n en Docker'
image: 'https://raw.githubusercontent.com/elcaza/misc/refs/heads/main/blog/sysadmin/n8n_docker/portada.jpg'
tags: [Sysadmin, Contenedores]
category: 'Sysadmin'
draft: false 
lang: 'es'
---

# Enlaces
+ <a href="https://docs.n8n.io/hosting/installation/docker/#starting-n8n" target="_blank">n8n en Docker</a>
+ <a href="https://groq.com/" target="_blank">Groq - Modelos de lenguaje</a>
+ <a href="https://console.cloud.google.com/" target="_blank">Google Cloud Console - Para conectar ejemplos con los servicios de Google (No requiere suscripción. Varios servicios tienen una capa gratuita)</a>

# Prepara una carpeta
~~~bash
mkdir n8n 
cd n8n
~~~

# Crear docker-compose.yml
`docker-compose.yml`
~~~bash
version: '3.8'

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n:latest
    container_name: n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=http://localhost:5678/
      - GENERIC_TIMEZONE=America/Mexico_City 
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:
    external: false
~~~
+ <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List" target="_blank">Cambia a tu zona horaria</a>

~~~bash
sudo docker compose up -d
~~~

+ <a href="http://localhost:5678" target="_blank">Panel n8n</a>

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