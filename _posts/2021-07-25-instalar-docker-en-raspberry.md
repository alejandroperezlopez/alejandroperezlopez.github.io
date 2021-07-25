---
layout: post
title: Cómo instalar Docker en tu Raspberry Pi de una manera sencilla
#image: https://farm8.staticflickr.com/7837/46516861382_db647a10a7_b.jpg
description: Guía rápida para instalar Docker en una Raspberry Pi (aplicable a cualquier distro de Linux)
category: tech
tags: ['2021', raspberry pi, linux, docker]
---

En este pequeño tutorial veremos cómo instalar Docker en nuestra Raspberry Pi (o cualquier servidor Linux) de una manera muy sencilla.

<!-- more -->

Lo primero que tendréis que hacer es aplicar este comando en la terminal. ¿Que qué es esto? Es un script creado por la propia Docker que instalará y configurará Docker en vuestro seridor.

```bash
sudo curl -sSL https://get.docker.com | sh
```

Para probarlo únicamente tendréis que ejecutar en una terminal y os responderá con los comandos disponibles:

```bash
docker
```

Ups, ¿cómo? ¿os está dando un error? Todo tiene explicación ;) Posiblemente el error esté relacionado con un tema de permisos. Para solucionarlo tendréis que añadir al grupo "docker" el usuario que estéis utilizando.

```bash
sudo groupadd docker
```

```bash
sudo usermod -aG docker $USER
```

Esta variable $USER se reemplazará en tiempo de ejecución por vuestro usuario actual. Ahora podemos volver a probar.

```bash
docker
```

¿Funciona? ¡Perfecto! Pues vamos a hacer una prueba. Ejecuta esto en tu terminal:

```bash
docker run hello-world
```

Este sencillo contenedor, hará un PULL de su imagen y publicará un sencillo mensaje informativo.

Pues ya estaría, como siempre, cualquier cosa déjamelo en los comentarios.