---
layout: ../../layouts/post.astro
title: "Programación directa sobre un módulo ESP8266"
pubDate: 2015-05-12
description: "Después de experimentar con el módulo ESP8266 y un arduino, ahora toca **programar directamente** sobre el módulo wifi ESP8266. Sí, este mód"
author: "akirasan"
isPinned: false
excerpt: "Después de experimentar con el módulo ESP8266 y un arduino, ahora toca **programar directamente** sobre el módulo wifi ESP8266. Sí, este mód"
image:
  src: ""
  alt: "Programación directa sobre un módulo ESP8266"
tags: ["Linux", "DIY", "IoT", "ESP8266"]
---

Después de experimentar con el módulo ESP8266 y un arduino, ahora toca **programar directamente** sobre el módulo wifi ESP8266. Sí, este módulo puede ser programado directamente, pero para ello no vale el firmware que viene por defecto (que acepta solo comandos AT), sino que hay que cargar un firmware específico diseñado para interpretar scripts, como por ejemplo [NodeMCU](http://nodemcu.com/), un firmware open source basado en el lenguaje de programación [Lua](http://www.lua.org/) y que podemos utilizar en nuestro ESP8266, ¿interesante, no?. El objetivo es poder realizar de forma sencilla creaciones IoT sin necesidad por ejemplo de un arduino, ya que podemos utilizar los pines GPIO (o general-purpose input/output) para conectar algún sensor o dispositivo. Lo mejor es ponernos al lío.

Vamos a hacer el primer *"hola mundo!!!"* con nuestro módulo ESP8266. Ojo que lo vamos a hacer supersencillo, solamente comunicando por Serial una instrucción :) . Como hemos comentado lo primero es el cambio del firmware:

###### Flasher módulo ESP8266

**Programa de flasheo**

* **Para Windows** por aquí <https://github.com/nodemcu/nodemcu-flasher>
* **Para Linux** por aquí tenéis una guía <http://www.whatimade.today/flashing-the-nodemcu-firmware-on-the-esp8266-linux-guide/> básicamente es utilizar una utilizad en Python llamada ***esptool.py*** y que podéis encontrar en <https://github.com/themadinventor/esptool> (necesitará la librería [pySerial](http://pyserial.sourceforge.net/) que se puede instalar mediante un `sudo apt-get install python-serial`). [Descarga esptool.py](https://github.com/themadinventor/esptool/raw/master/esptool.py)

**Firmware**

* La **última versión del firmware** está aquí: <https://github.com/nodemcu/nodemcu-firmware>. Aquí tenéis el enlace [directo a la última versión](https://github.com/nodemcu/nodemcu-firmware/raw/master/pre_build/latest/nodemcu_latest.bin)

###### Conexión módulo ESP8266

Bueno ya casi lo tenemos todo, falta conectar nuestro módulo ESP8266 al puerto USB, para ello utilizaremos un módulo FTDI o CP2102 y la siguientes conexiones:

| USB a Serial | ESP8266 |
| --- | --- |
| 3.3v | 3.3v, CH\_PD |
| GND | GND |
| Tx | Rx |
| Rx | Tx |
| GPIO0\* | GND\* |

\* **La conexión de GPIO0 a GND es requisito para la actualización del firmware.**

Para simplificarme la tarea me he montado una placa sencilla para reutilizarla en cualquier momento

![](/content/images/2015/05/IMG_20150509_224617.jpg)  
![](/content/images/2015/05/IMG_20150510_000855.jpg)

Para ver que responde utilizaremos un programa de comunicación por Serial, yo en Linux utilizo el programa **CuteCom**. La primera prueba sencilla es ver que nos contesta con el comando 'AT' y verificamos la versión con el comando 'AT+GMR'. Recordad que hay que conectarlo a 9600

![](/content/images/2015/05/Selecci-n_030.jpg)

Mi versión es la 0018000902-AI03, la nueva con este comando (revisa donde está conectado, en el ejemplo es el puerto ttyUSB0, y revisa la velocidad de conexión, para la versión que tiene este módulo en concreto es 9600):

```auto
sudo python ./esptool.py --port /dev/ttyUSB0 --baud 9600 write_flash 0x000000 nodemcu_latest.bin
```

Después de un rato, tendremos una salida por terminal de este estilo:

```auto
Connecting...
Erasing flash...
Writing at 0x00062000... (100 %)

Leaving...
```

Listo!!!,...si ahora nos conectamos como antes, mediante CuteCom y ejecutamos el comando `print 'hola mundo!!!'`ya tendremos nuestro primer *"hola mundo!!!"*, objetivo cumplido!!!

![](/content/images/2015/05/Selecci-n_032.jpg)

**Ahora ya no valen los comandos 'AT'**, solo programación mediante lua ;). Si queremos restaurar al firmware anterior podemos utilizar este que tengo, [os lo dejo aquí](http://akirasan.net/content/images/2015/05/v0.9.2.2_AT_Firmware.bin). Otra cosa, si queréis cambiar la velocidad de comunicación (baud) se tiene que hacer mediante el comando: `AT+CIOBAUD=<velocidad_baud>` (9600, 19200, 38400,...). También [aquí teneis PDF con los comandos AT](http://akirasan.net/content/images/2015/05/ESP8266ATCommandsSet.pdf) para la versión firmware que os he indicado.

**Siguiente objetivo:** conectar un sensor de temperatura directamente a un módulo ESP8266 y que podamos publicar su valor en nuestra red interna, y sin tener que utilizar un arduino!!!. Para ello vamos a hacer un script y cargarlo en el módulo wifi ESP8266 de tal forma que automáticamente se ejecute al realizar el boot.

###### Enlaces de interés

**API nodeMCU** Todos los métodos disponibles del firmware para ESP8266 <https://github.com/nodemcu/nodemcu-firmware/wiki/nodemcu_api_en>

Aquí podéis encontrar algunos tutoriales e información para comenzar a **aprender el lenguaje *lua***: <http://esp8266.co.uk/>

Una comunidad muy amplia donde consultar y resolver dudas en este foro [ESP8266 Community Forum](http://www.esp8266.com/)
