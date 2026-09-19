---
layout: ../../layouts/post.astro
title: "Cómo instalar MicroPython en un módulo ESP8266-12"
pubDate: 2016-07-02
description: "es una implementación de Python 3.X para microcontroladores y pequeños sistemas embebidos. MicroPyhton se fundó gracias a una campaña de Kic"
author: "akirasan"
isPinned: false
excerpt: "es una implementación de Python 3.X para microcontroladores y pequeños sistemas embebidos. MicroPyhton se fundó gracias a una campaña de Kic"
image:
  src: "/content/images/2016/07/upython-with-micro.jpg"
  alt: "Cómo instalar MicroPython en un módulo ESP8266-12"
tags: ["ESP8266", "python"]
---

[MicroPython](http://www.micropython.org/) es una implementación de Python 3.X para microcontroladores y pequeños sistemas embebidos. MicroPyhton se fundó gracias a una campaña de Kickstarter. Y es un software libre y abierto, bajo licencia MIT.

Me he comprado un par de estos módulos y he decidido probar uno de ellos con MicroPyhton y de paso compartir todo el proceso:

![](/content/images/2016/07/IMG_20160630_230831-1.jpg)

Conectamos a nuestro sistema por USB y miramos con `dmesg` en que puerto está disponible:

![](/content/images/2016/07/Selecci-n_001.png)

Recapitulemos de un par de entradas anteriores [ESPlorer: IDE de programación para ESP8266](www.akirasan.net/esplorer-ide-de-programacion-para-esp8266/) y [Programación directa sobre un módulo ESP8266](www.akirasan.net/programacion-directa-sobre-un-modulo-esp8266/), algunos de los componentes:

* [ESPlorer](https://github.com/4refr0nt/ESPlorer): Integrated Development Environment (IDE) para desarrolladores de ESP8266
* [esptool.py](https://github.com/themadinventor/esptool/): Utilidad en Python para comunicar con el bootloader de la ROM del Espressif ESP8266. De fácil instalación mediante `pip install esptool`
* [MicroPython port para ESP8266](https://github.com/micropython/micropython/tree/master/esp8266): MicroPython port para ESP8266 es en versión experimental para los modulos WiFi basados en el chip ESP8266 Espressif.

###### ¿Qué vamos a necesitar una vez conectada nuestra placa de desarrollo ESP8266-12E?

Estos serían los pasos a seguir:

1. Construir el firmware con MicroPython para ESP8266  
   1.1 Compilar OpenSource ESP SDK  
   1.2 Compilar firmware MicroPython
2. Flashear nuestro módulo ESP8266
3. Test *"hola mundo!!!"*

##### Construir el firmware MicroPython

###### Compilar OpenSource ESP SDK

Para compilar nuestro firmware **vamos a necesitar OpenSource ESP SDK**, el cual se puede encontrar aquí <https://github.com/pfalcon/esp-open-sdk>. Pero vamos a tratar de simplificar con este comando todo lo que necesitamos:

```auto
sudo apt-get install make unrar autoconf automake libtool gcc g++ gperf flex bison texinfo gawk ncurses-dev libexpat-dev python-dev python python-serial sed git unzip bash help2man wget bzip2 libtool-bin binutils-dev newlib-source
```

Clonamos el repositorio de OpenSource ESP SDK en GitHub:

```auto
git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
```

Una vez clonado, nos movemos al directorio `esp-open-sdk` y desde ahí vamos a ejecutar el `make STANDALONE=y` y nos iremos a preparar un café...hay cómo para mas de 30 minutos:

```auto
cd esp-open-sdk
make STANDALONE=y
```

...después de estos agradables momentos del `make` y si no te ha dado ningún error...para mi ha sido un auténtico calvario!!!, por eso en la instalación de los paquetes he añadido todo aquello que me he encontrado

Muchos errores han sido de este estilo (puedes encontrar el detalle en fichero `build.log` que se encuentra en el directorio `/esp-open-sdk/crosstool-NG/`):

```auto
[ERROR]  
[ERROR]  >>
[ERROR]  >>  Build failed in step 'Retrieving needed toolchain components' tarballs'
[ERROR]  >>        called in step '(top-level)'
[ERROR]  >>
[ERROR]  >>  Error happened in: do_binutils_get[scripts/build/binutils/binutils.sh@741]
[ERROR]  >>        called from: main[scripts/crosstool-NG.sh@592]
[ERROR]  >>
[ERROR]  >>  For more info on this error, look at the file: 'build.log'
[ERROR]  >>  There is a list of known issues, some with workarounds, in:
[ERROR]  >>      'share/doc/crosstool-ng/crosstool-ng-1.22.0-55-gecfc19a/B - Known issues.txt'
[ERROR]  
[ERROR]  (elapsed: 23:26.03)
```

Por ejemplo, en éste se queja de que no encuentra el paquete `binutils` (que la he incluido en la línea de paquetes a instalar) y así unas cuentas.

Finalmente tendremos una salida cómo ésta:

```auto
Xtensa toolchain is built, to use it:

export PATH=/home/akirasan/esp-open-sdk/xtensa-lx106-elf/bin:$PATH

Espressif ESP8266 SDK is installed, its libraries and headers are merged with the toolchain
```

Ahora para que *toolchain* `xtensa-lx106-elf-gcc` (la herramienta de compilación que se ha generado) funcione cuando compilemos nuestro firmware de MicroPython, debemos añadirlo a nuestra variable de entorno `PATH`. Dentro del directorio `esp-open-sdk` donde hemos ejecutado el `make`:

De forma temporal, en cuanto hacemos logout lo perdemos sería así:

```auto
PATH=$PATH:$(pwd)/xtensa-lx106-elf/bin
export PATH
```

Para hacerlo de forma permanente sería algo así:

```auto
echo "PATH=$(pwd)/xtensa-lx106-elf/bin:\$PATH" >> ~/.profile
```

###### Compilar firmware

Se supone que ésta será la parte más sencilla. Veamos. Lo primero va a ser clonar el repositorio GitHub de `micropython`:

```auto
git clone https://github.com/micropython/micropython
```

Entramos en el directorio de `micropython` y vamos a añadir las posibles dependencias que tiene MicroPython, y vamos a pre-compilar algunos scripts:

```auto
cd micropython
git submodule update --init
make -C mpy-cross
```

Luego entramos en el directorio de `esp8266` donde seguidamente vamos a compilar nuestro firmware:

```auto
cd esp8266
make axtls
make
```

Una vez finalizada la compilación tendremos un fichero *.bin* llamado `firmware-combined.bin` en el directorio `\build`, éste será nuestro firmware de MicroPython.

##### Flashear nuestro módulo ESP8266

La parte mas sencilla (o por lo menos eso me ha parecido a mí). Simplemente desde el directorio `/build` que es donde tenemos nuestro fichero firmware, ejecutamos el `esptool.py` (recuerda indicar al parámetro `-p` el puerto donde está conectado el módulo ESP8266, en mi caso está en `/dev/ttyUSB0`):

```auto
esptool.py -p /dev/ttyUSB0 write_flash --flash_size=8m 0 firmware-combined.bin
```

Mi ejecución ha sido algo así:

```auto
esptool.py v1.2-dev
Connecting...
Running Cesanta flasher stub...
Flash params set to 0x0020
Writing 512000 @ 0x0... 512000 (100 %)
Wrote 512000 bytes at 0x0 in 44.4 seconds (92.3 kbit/s)...
Leaving...
```

##### Test *"hola mundo!!!"*

Vamos a ver y comprobar si todo ha funcionado como lo esperado. Para ello vamos ha conectarnos a nuestro módulo ESP8266 con el firmware de MicroPython y ejecutar un *hola mundo!!!*.

Cómo hemos subido un firmware nuevo, lo ideal es hacer un reset al módulo para que vuelva a hacer el boot. Luego podemos utilizar cualquier software para conectarnos por el puerto COM, yo he utilizado *CuteCom*:

![esp8266 micropython](/content/images/2016/07/Selecci-n_003.png)

Cómo se puede ver en la captura, una vez conectados podemos lanzar sentencias Python para que ejecute.

Hay librería implementadas para MicroPython que nos ayudarán en el desarrollo. Ejemplos y últimas versiones, cómo siempre en la página de MicroPython.

Pero **si no queréis complicaros mucho** y simplemente probarlo [podéis utilizar mi compilación de fecha Julio 2016](https://github.com/akirasan/micropython_esp8266) que he subido a [mi GitHub](https://github.com/akirasan) o por ejemplo la que tiene publicada [Adafruit.com en su web](https://learn.adafruit.com/building-and-running-micropython-on-the-esp8266/build-firmware), algo mas viejuna: [Adafruit --- MicroPython firmware ESP8266 5/12/2015](https://s3.amazonaws.com/adafruit-download/MicroPython_ESP8266_Firmware_5_12_2015.zip).

Lo ideal es que si pasa tiempo, intentéis compilar un firmware actualizado, porque cómo he comentado al inicio, es un software experimentar y es bueno tener las últimas correcciones.
