---
layout: post
title: Configurar IP estática en Raspberry Pi
category: tech
tags: [raspberry pi, ip estatica, raspbian, static ip, jessie]
---

En este pequeño post vamos a ver cómo configurar IP estática tanto para conexiones WIFI como cableadas.

<!-- more -->

Lo único que tenéis que hacer es editar el fichero __/etc/dhcpcd.conf__ con vuestro editor favorito y añadir __al final del fichero__ una de las siguientes configuraciones (o ambas) en función de si queréis usar WIFI / Ethernet.

En ambos casos, tenéis que sustituir _IP_DESEADA_ e _IP_DE_VUESTRO_ROUTER_ por la IP que queréis que tenga vuestra RaspberryPi y la IP de vuestro router dentro de la red local respectivamente.

En la sección _domain_name_servers_ configuraremos las IPs de los dns de Google (que dan muy buen resultado).

## Configurando IP estática para conexiones WIFI

```text
    interface wlan0

    static ip_address=IP_DESEADA/24
    static routers=IP_DE_VUESTRO_ROUTER
    static domain_name_servers=8.8.8.8 8.8.4.4
```

## Configurando IP estática para conexiones por cable ethernet.

```text
    interface eth0

    static ip_address=IP_DESEADA/24
    static routers=IP_DE_VUESTRO_ROUTER
    static domain_name_servers=8.8.8.8 8.8.4.4
```

Podéis comentarme cualquier duda o problema que tengáis con esta configuración en mi cuenta de Twitter [@alexperezl](https://twitter.com/alexperezl).