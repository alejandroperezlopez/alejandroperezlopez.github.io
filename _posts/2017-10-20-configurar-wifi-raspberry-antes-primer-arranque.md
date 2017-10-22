---
layout: post
title: Configurar la conexión wifi de tu Raspberry Pi antes del primer arranque
category: tech
tags: [raspberry pi, wifi, raspbian, headless, jessie]
---

Uno de los mayores incordios que nos hemos encontrado todos al configurar una nueva Raspberry Pi es la necesidad de conectarla a algún monitor / teclado para configurar la __conexión wifi__ aunque esa máquina esté destinada a ser headless. 

Esto ya no es un problema desde Mayo de 2016, momento en el que la fundación [Raspberry](https://www.raspberrypi.org/) introdujo la posibilidad de configurar la red wifi directamente desde la SD antes del primer arranque.

<!-- more -->
En este tutorial, veremos cómo, paso por paso.

## Configurando nuestra conexión 

Dentro de la carpeta _boot_ de nuestra SD creamos un fichero con nombre __wpa_supplicant.conf__ con el contenido de una de las dos siguientes opciones:

1. Utilizar directamente la contraseña de tu WIFI en plano:

```text
country=GB
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="SSID_DE_TU_WIFI"
    password="PASSWORD_DE_TU_WIFI"
}
```

2. Utilizando una passphrase:

```bash
wpa_passphrase "SSID_DE_TU_WIFI" "PASSWORD_DE_TU_WIFI"
```

La salida de este comando será algo como:

```text
network={
    ssid="SSID_DE_TU_WIFI"
    #psk="PASSWORD_DE_TU_WIFI"
    psk=TUPSK
}
```

Simplemente tienes que copiar la salida del comando anterior en tu fichero __wpa_supplicant.conf__ (puedes eliminar la línea comentada con # para eliminar completamente tu password en claro del fichero)

```text
country=GB
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="SSID_DE_TU_WIFI"
    psk="PASSWORD_DE_TU_WIFI"
}
```

Personalmente, suelo utilizar esta última opción ya que me parece más robusta.

## Activando SSH

Desde Septiembre de 2016 y, [alegando motivos de seguridad](https://www.raspberrypi.org/documentation/remote-access/ssh/), __la conexión SSH está desactivada por defecto__ en Raspbian, veamos cómo activar de nuevo esta característica antes del primer arranque:

Únicamente debemos copiar en la carpeta _boot_ de nuestra raspberry un fichero con nombre __ssh__ (sin extensión). Cuando arranquemos nuestra Rasberry Pi, tendremos el ssh activado. Recordad que el usuario por defecto es:

```text
usuario: pi
contraseña: raspberry
```














