---
layout: post
title: Comprobar temperatura CPU / GPU Raspberry Pi
category: tech
tags: [raspberry pi, temperatura, raspbian]
---

En este pequeño tutorial veremos cómo medir la temperatura de funcionamiento de una Raspberry Pi, tanto de su CPU como de su GPU, siento esta actualizada en tiempo real hasta que cerremos el script.

<!-- more -->

## Creando nuestro script

Crea un fichero con nombre _temp.sh_ y copia en su interior el contenido de este script:

```bash
nano temp.sh

```

```bash
#!/bin/bash

while true
do
  cpuTemp0=$(cat /sys/class/thermal/thermal_zone0/temp)
  cpuTemp1=$(($cpuTemp0/1000))
  cpuTemp2=$(($cpuTemp0/100))
  cpuTempM=$(($cpuTemp2 %$cpuTemp1))
  clear
  echo "CPU temp=$cpuTemp1.$cpuTempM'C"
  echo "GPU $(/opt/vc/bin/vcgencmd measure_temp)"
  sleep 1
done
```

El script debe tener permisos de ejecución, si no lo has hecho ya, dale permisos con el siguiente comando:

```bash
chmod +x temp.sh
```

Únicamente nos queda ejecutar el script y comprobar la temperatura de nuestra Raspi.

```bash
sh temp.sh
```

La salida debería ser similar a esta (irá cambiando mientras no pulses Control + C, en ese momento la ejecución del script se detendrá) :

```
CPU temp=58.5'C
GPU temp=58.5'C
```

¡Espero que os sirva de ayuda!