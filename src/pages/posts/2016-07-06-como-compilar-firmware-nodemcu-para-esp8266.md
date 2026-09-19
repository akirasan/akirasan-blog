---
layout: ../../layouts/post.astro
title: "Cómo compilar firmware NodeMCU para ESP8266"
pubDate: 2016-07-06
description: "Otra entrada dedicada a este módulo WiFi tan impresionante, versátil y económico. En este caso utilizaremos el firmware de , pero vamos a co"
author: "akirasan"
isPinned: false
excerpt: "Otra entrada dedicada a este módulo WiFi tan impresionante, versátil y económico. En este caso utilizaremos el firmware de , pero vamos a co"
image:
  src: "/content/images/2016/07/nodemcu-logos.png"
  alt: "Cómo compilar firmware NodeMCU para ESP8266"
tags: ["ESP8266", "nodemcu"]
---

Otra entrada dedicada a este módulo WiFi tan impresionante, versátil y económico. En este caso utilizaremos el firmware de [NodeMCU](http://nodemcu.com/index_en.html), pero vamos a compilar el código fuente con las últimas actualizaciones liberadas en el [GitHub de NodeMCU](https://github.com/nodemcu).

Igualmente podemos seguir los pasos que están descritos en la [wiki de NodeMCU](https://nodemcu.readthedocs.io/en/dev/en/build/). Pero yo voy a explicar los problemas que me he encontrado por el camino ;)

Lo primero que vamos a hacer darnos una vuelta la esta entrada del blog [Cómo instalar MicroPython en un módulo ESP8266-12](http://www.akirasan.net/como-instalar-micropython-esp8266-12/) donde hay una primera parte en la que se explica cómo compilar **OpenSource ESP SDK** *toolchain xtensa-lx106-elf-gcc*, **requisito para poder compilar** cualquier firmware para ESP8266, entre otros éste de NodeMCU.

Una vez tenemos *toolchain* de OpenSource ESP SDK (recordad que en la varible PATH debemos tener la ruta donde está *xtensa-lx106-elf-gcc*), ya podemos clonar el repositorio de NodeMCU:

```auto
git clone https://github.com/nodemcu/nodemcu-firmware.git
```

Ahora cambiamos al directorio `nodemcu-firmware` que nos ha creado `git` y ejecutamos `make`:

```auto
cd nodemcu-firmware
make
```

Una vez ha finalizado en la carpeta `bin` vamos a tener dos archivos:

```auto
0x00000.bin
0x10000.bin
```

Estos serán los que utilicemos para flashear nuestro módulo. Para ello vamos a utilizar el siguiente comando (si lo ejecutamos desde el directorio donde hemos realizado el `make` ya hace referencia a los anteriores ficheros .bin. Recordad también cambiar el parámetro donde se indica el puerto de conexión `/dev/ttyUSB0`)

```auto
esptool.py -p /dev/ttyUSB0 write_flash 0x00000 ./bin/0x00000.bin 0x10000 ./bin/0x10000.bin
```

Una vez ha finalizado, lo ideal es hacer un reset del módulo. Para verificar que tenemos nuestro módulo funcionando en NodeMCU, lo que tenemos que hacer es conectarnos al puerto COM y cuando estamos conectados hacerle un reset, veremos entonces como se inicia y nos devuelve la información de la versión:

![nodemcu esp8266](/content/images/2016/07/nodemcu_esp8266.png)

En el momento que he creado la entrada es la **NodeMCU 1.5.1 by Lua 5.1.4 con SDK 1.5.1**

En [mi GitHub](https://github.com/akirasan/nodemcu_esp8266) he dejado los ficheros `0x00000.bin` y `0x10000.bin` por si queréis utilizarlos y no liaros con la parte de la compilación.

**ACTUALIZADO 23/07/2016**

Hay un servicio online que te permite configurar y te compila el firmware a tu gusto ;) <http://nodemcu-build.com/index.php>
