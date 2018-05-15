---
layout: post
title: Cómo minar criptomonedas con tu Raspberry Pi y por qué no deberías hacerlo
image: https://c1.staticflickr.com/1/967/39870627670_a261810d15_o.jpg
description: Te enseñamos cómo minar criptomonedas con tu Raspberry Pi y veremos por qué no es rentable y deberías tomártelo como un experimento.
category: tech
tags: ['2018', 'criptomonedas', 'raspberry pi', 'smc', 'magi', 'magicoin', 'xmg']
---

Desde hace un par de años hay una palabra que está muy presente en noticiarios e incluso conversaciones familiares, y es [bitcoin](https://es.wikipedia.org/wiki/Bitcoin){:target="_blank"}. Aunque es la más famosa existen un incontable número de [criptomonedas](https://www.bbva.com/es/criptomonedas-sirven-las-monedas-virtuales/){:target="_blank"}. Pero, ¿qué es entonces una __criptomoneda__?

Una criptomoneda no es más que __una forma de intercambio__ en una red llamada __cadena de bloques__ donde se garantiza la fiabilidad e integridad de todas las operaciones. Esta garantía se produce mediante el __minado__ que no es más que la realización de una serie de operaciones matemáticas para verificar cada operación. A cambio de esta capacidad de procesamiento obtienes lo que se llama __proof of work__ que es una pequeña recompensa en la moneda asociada a la red a la cual estamos dando nuestra capacidad de procesamiento.

Pero ¿por qué están tan de moda las criptomonedas?

<!-- more -->

### Por qué las criptomonedas están de moda

En primer lugar podemos decir que se han popularizado debido a su volatilidad y a la posibilidad de obtener beneficios en cortos períodos de tiempo. En la siguiente imagen se puede apreciar el precio histórico del Bitcoin.

<img src="https://farm1.staticflickr.com/965/26844167137_a211090efd_o.png" alt="Gráfico histórico de precio de Bitcoin" class="img-fluid">

Como se puede ver el precio desde Octubre de 2013 ha pasado de prácticamente USD 0 a USD 20000 para luego mantenerse en unos ~ USD 10000. Si has comprado (o minado) un Bitcoin en Octubre de 2013 tu inversión se habría __multiplicado por 20000__. Es un pastel demasiado goloso.

### ¿Qué es minar una criptomoneda?

Como decíamos en el comienzo de esta entrada, hay dos formas de conseguir un token (que es el nombre técnico que recibe cada criptomenda), __comprándolo en los diferentes exchanges__ (que funcionan más o menos como una casa de cambio de dinero normal, o __fiat__) y la otra es mediante el proceso de __minado__.

Se denomina __minado__ a la actividad por la cual se emiten nuevas divisas (o fracciones de la misma) de cierto token y a la vez se produce la confirmación de transacciones en una red blockchain. Existen diferentes tipos de minado pero en esta artículo nos centraremos en dos, __prueba de trabajo (PoW)__ y __prueba de participación (PoS)__.

##### Pueba de trabajo (PoW)

En este sistema la persona que realiza el minado ofrece a la red su potencia de procesamiento (que será usada para generar un nuevo bloque donde se confirmarán N transacciones). Por cada nuevo bloque se generan N token nuevos, que son repartidos entre los miembros que han colaborado en dicho minado de forma proporcional a la potencia de procesado que han aportado. 

##### Pueba de participación (PoS)

En este proceso, las transacciones son procesadas y confirmadas probando la posesión de una cierta cantidad de tokens de la criptomoneda en cuestión. Por ejemplo, si posees 1024 tokens de una moneda X, tendrás un PoS mayor que otro usuario que posea 20 tokens.

### Qué criptomonedas podemos minar con nuestra Raspberry Pi

Como sabemos nuestras amadas Raspberry Pi no destacan por su enorme capacidad de cálculo. La mayoría de criptomonedas se minan actualmente mediante GPU (el minado por CPU de ese mismo token llevaría muchísimo más tiempo) o ASICs, estos ASIC son dispositivos especialmente diseñados para minar usando un cierto algoritmo por lo que su eficiencia es muy superior.

Los creadores de varias criptomonedas están vetando el uso de estos ASIC mediante cambios en el algoritmo para evitar su uso y que el minado funcione de forma más equitativa y con una menor dificultad.

Debido a las características de nuestras Raspberry Pi, debemos buscar una criptomoneda que esté optimizada para ser minada mediante CPU y a poder ser optimizada para dispositivos de baja potencia. El candidato ideal es [MagiCoin (XMG)](http://www.m-core.org/){:target="_blank"}.

Magi (XMG) es una criptomoneda bastante atípica debido a que intenta evitar la competitividad a la que suele estar relacionada la actividad de __minado__ debido a que se penalizan las grandes potencias de minado. Con Magi, más no es siempre mejor.

### Creando un monedero virtual (wallet)

En este apartado vamos a ver cómo crear una monedero para la criptomoneda MagiCoin totalmente funcional en nuestra Raspberry Pi.

Si partimos de una instalación limpia, os recuerdo que en este mismo blog tenemos varios artículos explicando conceptos básicos:

- [Comprobar temperatura Raspberry Pi]({{ site.baseurl }}{% post_url 2017-10-18-comprobar-temperatura-raspberry-pi %}){:target="_blank"}

- [Cómo configurar wifi Raspberry Pi antes del primer arranque]({{ site.baseurl }}{% post_url 2017-10-20-configurar-wifi-raspberry-antes-primer-arranque %}){:target="_blank"}

- [Configurar IP estática en tu Rasperry Pi (cable y wifi)]({{ site.baseurl }}{% post_url 2017-10-23-Configurar-ip-estatica-raspberry-pi %}){:target="_blank"} 

Necesitaremos más adelante conocer el tipo de arquitectura que usa nuestra Raspberry Pi, para ello teclearemos:

```bash
uname -a
```

Debemos fijarnos en las dos últimas palabras que aparecen en el resultado de esta ejecución, pueden ser tal que __armv7l GNU/Linux__ o __armv61 GNU/Linux__. 
Nos interesa la parte referente a __arm*__. Nos guardamos este valor para más adelante.

##### Preparación

En primer lugar, para compilar nuestro __wallet__ necesitamos aumentar el tamaño de la memoria que nuestra Raspberry Pi asigna a funciones de swap.

```bash
sudo nano /etc/dphys-swapfile
```

Y modifica la línea que dice __CONF_SWAPSIZE=100__ con un valor de __CONF_SWAPSIZE=2048__ . Una vez modificado pulsamos Control + X y a continuación la letra Y y Enter.

A continuación tecleamos el siguiente comando:

```bash
sudo dphys-swapfile setup && sudo dphys-swapfile swapon
```

Si usamos el comando _free -m_ antes y después podremos ver que ahora tenemos más espacio asignado a swap.

Instalaremos a continuación las dependencias requeridas:

```bash
sudo apt install -y git libdb-dev libboost-all-dev libminiupnpc-dev libgmp-dev libdb5.3++-dev screen
```

Ahora tenemos que hacer un alto en el camino ya que necesitamos conocer la versión de Raspbian que usamos, para ello tecleamos:

```bash
cat /etc/os-release
```

En el resultado de dicho comando debemos buscar la línea referente a __VERSION__ ahí veremos si aparece _jessie_ o _stretch_.

__Si tenemos Jessie__

Simplemente ejecutaremos:

```bash
sudo apt-get install libssl-dev
```

__Si tenemos Stretch__

Ejecutamos el siguiente comando para eliminar la librería si previamente la teníamos instalada:

```bash
sudo apt-get remove libssl-dev
```

Editaremos nuestro _sources.list_ de forma que apunte de forma temporal a los repositorios de Jessie.

```bash
sudo nano /etc/apt/sources.list
```

Actualizamos la lista de dependencias ejecutando:

```bash
sudo apt-get update
```
Y con el siguiente comando revisamos cual es la librería candidata a ser instalada:

```bash
sudo apt-cache policy libssl-dev
```

Nos saldrá algo similar a:

```
libssl-dev:
  Installed: (none)
  Candidate: 1.0.1t-1+deb8u7
  Version table:
     1.0.1t-1+deb8u7 0
        900 http://mirrordirector.raspbian.org/raspbian/ jessie/main armhf Packages
```

Instalamos el paquete mediante el comando:

```bash
sudo apt_get install libssl-dev
```

Finalmente volveremos a editar nuestro fichero _sources.list_ de forma que vuelva a apuntar a los repositorios de Stretch.

##### Compilación y configuración de nuestro wallet

Nos descargamos el código fuente mediante el comando:

```bash
git clone https://github.com/magi-project/magi.git
```

Cuando termine el comando tendremos una carpeta _magi_ donde estará almacenado el código fuente. A continuación ejecutamos:

```bash
cd magi/src && make -f makefile.unix xCPUARCH={VALOR_DE_NUESTRA_ARQUITECTURA}
```
__ATENCIÓN__ : En el comando anterior debemos reemplazar {VALOR_DE_NUESTRA_ARQUITECTURA} (incluyendo los {}) por el valor de arquitectura que obtuvimos en el primer paso de la sección de prepración.
__NOTA__ : Recomendamos el uso de screen para ejecutar este comando, debido a que puede llevarle bastante tiempo si quieres conocer más datos acerca de cómo funciona screen puedes visitar este [tutorial de uso de screen](https://phenobarbital.wordpress.com/2013/02/18/linux-usando-gnu-screen/){:target="_blank"}

Cuando la compilación haya terminado veremos que en la carpeta _magic/src_ un fichero llamado _magid_ que será nuestro ejecutable. Con el siguiente usuario instalaremos el ejecutable:

```bash
cd ~/magi/src
sudo install -m 755 magid /usr/bin/magid
```

##### Descargamos el blockchain

Este paso no es realmente necesario ya que el propio wallet se puede poner al día en cuanto lo arranquemos pero le llevará un montón de tiempo así que es mejor darle un punto de partida más o menos actual para que únicamente tenga que actualizarlo.

Creamos una carpeta _blockchain_ :

```bash
cd ~~
mkdir blockchain
```

Nos descargamos el último blockchain disponible desde [https://m-core.org/bin/block-chain/](https://m-core.org/bin/block-chain/){:target="_blank"}, a día de hoy sería _m-block-chain-1813042.tar.gz_:

```bash
wget https://m-core.org/bin/block-chain/m-block-chain-1813042.tar.gz
```

Y lo descomprimimos:

```bash
tar xvf m-block-chain-1813042.tar.gz
mv ~/m-block-chain-1813042/* ~/blockchain/
rm -rf ~/m-block-chain-1813042
```

##### Configuramos nuestro wallet

Creamos una carpeta _.magi_ y dentro de él creamos un fichero vacío _magi.conf_

```bash
mkdir ~/.magi
touch ~/.magi/magi.conf

```

Ahora edita este nuevo fichero con tu editor favorito y añadimos este contenido al fichero:

```bash
# You must set rpcuser and rpcpassword to secure the JSON-RPC api
rpcuser=magirpc
rpcpassword=7dhf8SRjR8R5P9icfqjjbqAZqysYXeQSuX539JhXseLz
# Listen for RPC connections on this TCP port:
rpcport=8232
# server=1 to accept JSON-RPC commands
server=1
# listen=1 to accept connections from outside
listen=1
# RPC connection from localhost allowed
rpcallowip=127.0.0.1
# Add nodes to connect to specific peers
addnode=104.128.225.215
addnode=45.35.251.73
# posii=1 to enable PoS staking; default = 0
posii=1
# Transaction under stake with a value greater than the threshold is being splitted
stakesplitthreshold=500
# Transactions with values less than the threshold will combine into one
stakecombinethreshold=500
```

##### Arrancamos nuestro wallet por primera vez

Ejecutaremos:

```bash
/usr/bin/magid -daemon -conf=/home/pi/.magi/magi.conf -datadir=/home/pi/blockchain
```

Esta ejecución inicial podría tomar un poco más de tiempo ya que debe sincronizarse con el estado actual del blockchain. ¿Cómo sabremos cuándo ha terminado? Si ejecutas:

```bash
magid getinfo
```

Y no te devuelve algo como _Couldn't connect to server_ es que está listo :P

Pero esto no es usable, es decir, no podemos ejecutar nuestro servicio cada vez que arranquemos nuestra amada Raspberry Pi. Vamos a configurar un servicio que lo haga por nosotros:

```bash
sudo vim /lib/systemd/system/magid.service
```

En el cual añadimos:

```bash
[Unit]
Description=Coin Magi daemon services
After=tlp-init.service

[Service]
PIDFile=/home/pi/.magi/magi.pid
RemainAfterExit=yes
ExecStart=/usr/bin/magid -daemon -conf=/home/pi/.magi/magi.conf -datadir=/home/pi/blockchain
Restart=on-failure
RestartSec=15
User=pi

[Install]
WantedBy=multi-user.target
```

Activamos el autorun tecleando la siguiente instrucción:

```bash
sudo systemctl enable magid.service
sudo reboot
```

Y comprobamos el estado del servicio después de reiniciar:

```bash
systemctl status magid
```

Una pequeña guía de cómo usar los comandos de systemctl [aquí](https://gist.github.com/adriacidre/307d2f9f5179fc748f22edac5af3d218){:target="_blank"}

Y listo, ya tenemos nuestro wallet.

Es muy importante escriptar tu wallet para ello debes utilizar el siguiente comando (y recordar la contraseña que uses):

```bash
magid encryptwallet TUCLAVE
```

### Configurando nuestro primer minero

__NOTA:__ Recomiendo encarecidamente ejecutar este proceso en otra Raspberry Pi diferente (si tenemos varias) o en cualquier otro dispositivo (windows, linux, osx etc). Dado que el dispositivo va a estar sometido a una gran carga de CPU que hará que se comporte de forma algo inestable.

##### Si usas cualquier cosa que no sea una Raspberry Pi o algún dispositivo ARM

Descárgate uno de los binarios previamente compilados desde [aquí](https://github.com/magi-project/m-cpuminer-v2/releases){:target="_blank"} y conéctate a tu Pool de minado favorito. Si no sabes qué es un pool por favor pásate por [este artículo](https://www.oroyfinanzas.com/2015/08/evolucion-mineria-bitcoin-5-que-es-minar-pools/){:target="_blank"} que lo explica a la perfección.

##### Si usas una Raspberry Pi

Creamos un directorio llamado _minero_ y descargamos ahí el código fuente del minero desde GitHub:

```bash
mkdir minero
cd minero
git clone https://github.com/magi-project/m-cpuminer-v2.git
```

Nos situamos en la carpeta _m-cpuminer-v2_ y ejecutamos los siguientes comandos para instalar todas las dependencias:

```bash
sudo apt-get install libcurl4-openssl-dev libcurl4-nss-dev libgmp-dev libssl-dev libjansson-dev automake
```

Una vez hecho, debemos editar el fichero _Makefile.am_ buscaremos la línea que haga referencia a _-march=native_ y la cambiaremos por _mfpu=neon_.

```bash
sh autogen.sh
./configure CFLAGS="-mfpu=neon -mfloat-abi=hard"
make
```

Cuando termine de compilar (le llevará un buen rato), tendremos nuestro ejecutable correctamente compilado y listo para ser ejecutado.

##### Pools recomendados

Durante los aproximadamente 3 meses que he estado minando con éxito, lo he estado haciendo tanto en [M-Hash Pool](https://pom.m-hash.com/){:target="_blank"} como en [Minerclaim](https://xmg.minerclaim.net/){:target="_blank"} ambos son fiables y totalmente seguros.

### Por qué no debemos minar con nuestra Raspberry Pi

Si has llegado hasta aquí, debo darte las gracias por leerte todo este tocho. En este post he utilizado referencias de otros posts como [este](https://steemit.com/cryptocurrency/@gridcat/how-to-install-coin-magi-wallet-on-your-raspberry-pi){:target="_blank"} y [este](https://bitcointalk.org/index.php?topic=735170.msg20408200#msg20408200){:target="_blank"} gracias a sus autores he podido configurar correctamente mi wallet y mis mineros.

Como decía antes, he estado minando como 3 meses y he obtenido apenas $5 de beneficio (sin descontar obviamente el consumo de luz de los dispositivos), ahora mismo no resulta rentable debido a que en todos los mining pools hay personas u organizaciones aportando grandes cantidades de hashrate que hace que los dispositivos pequeños (incluso usando monedas diseñadas para favorecerlos) no tengan demasiado éxito.

Habrá que esperar a cómo evolucionan los mercados.

Espero que os haya gustado este post (también conocido como el wall-of-text), por favor dejarme en comentarios vuestros comentarios y si tenéis experiencia con cualquier otro tipo de minado o con otros dispositivos.




