---
layout: ../../layouts/post.astro
title: "Preparando Arduino IDE para ESP32+LoRa"
pubDate: 2018-05-09
description: "He de reconocer que ha sido algo *doloroso* el poder acabar hacer funcionar el IDE de Arduino con estos dos módulos LoRa con ESP32, por eso"
author: "akirasan"
isPinned: false
excerpt: "He de reconocer que ha sido algo *doloroso* el poder acabar hacer funcionar el IDE de Arduino con estos dos módulos LoRa con ESP32, por eso"
image:
  src: "/content/images/2018/05/IMG_1jqbqq.jpg"
  alt: "Preparando Arduino IDE para ESP32+LoRa"
tags: ["LoRa", "LoRaWan", "Arduino", "ESP32"]
---

He de reconocer que ha sido algo *doloroso* el poder acabar hacer funcionar el IDE de Arduino con estos dos módulos LoRa con ESP32, por eso he querido documentar los pasos y errores que me he encontrado. **--> Actualizado!!!, ahora no parece ser tan *doloroso***

![esp32_lora](/content/images/2018/05/IMG_prh4o.jpg)

Primero vamos a preparar el IDE de Arduino para que pueda compilar código para la placa ESP32. Tenemos que visitar el [repositorio de **espressif** en Github](https://github.com/espressif/arduino-esp32) donde encontraremos todo lo necesario:

![Selecci-n_153](/content/images/2018/05/Selecci-n_153.png)

**Actualizado** [6/12/2018]

~~En la *guía de instalación* dejan más o menos claro cómo hacerlo. Digo más o menos, porque hay un par de detalles que no lo explican y os dará error. Pero vamos por parte y [siguiendo la guia](https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/debian_ubuntu.md), primero ejecutamos una serie de comandos para bajar e instalar los componentes.  
![Selecci-n_154](/content/images/2018/05/Selecci-n_154.png)  
En mi caso, algunos de los primeros pasos me los he ahorrado porque ya los tenía realizados anteriormente.  
![Selecci-n_145](/content/images/2018/05/Selecci-n_145.png)  
Hasta aquí y según la guía ya está todo listo para arrancar el IDE de Arduino y disfrutar,...pues no!!!. Yo me he encontrado con varios problemas.~~

Parece ser que **la instalación ahora es mucho mas sencilla** que hace unos meses. Simplemente en el menú de *Preferencias* vamos a añadir la dirección al json que instala toda la info del micro ESP32: <https://dl.espressif.com/dl/package_esp32_index.json>

![Selecci-n_494](/content/images/2018/12/Selecci-n_494.png)

![Selecci-n_495](/content/images/2018/12/Selecci-n_495.png)

Dejo a continuación los problemas que había encontrado anteriormente por si pueden servir a alguien. Pero está claro que con esta actualización pueden haberse quedado desfasados.

## Problema 1. *No se pudo encontrar boards.txt*

Éste problema lo veremos en el momento que vayamos a seleccionar desde el menú de *Herramientas* -> *Placa ""*, ya que en ese momento va a buscar información sobre las placas que tenemos instaladas, y la de esp32 no se ha instalado bien:

![Selecci-n_149](/content/images/2018/05/Selecci-n_149.png)

```auto
No se pudo encontrar boards.txt en /home/akirasan/Arduino/hardware/esp32/tools. Es pre-1.5?
No se pudo encontrar boards.txt en /home/akirasan/Arduino/hardware/esp32/libraries. Es pre-1.5?
No se pudo encontrar boards.txt en /home/akirasan/Arduino/hardware/esp32/variants. Es pre-1.5?
No se pudo encontrar boards.txt en /home/akirasan/Arduino/hardware/esp32/package. Es pre-1.5?
No se pudo encontrar boards.txt en /home/akirasan/Arduino/hardware/esp32/docs. Es pre-1.5?
No se pudo encontrar boards.txt en /home/akirasan/Arduino/hardware/esp32/cores. Es pre-1.5?
WARNING: Error loading hardware folder /home/akirasan/Arduino/hardware/esp32
  No se encontraron definiciones de hardware validas en la carpeta esp32.
WARNING: Error loading hardware folder /home/akirasan/Arduino/hardware/espressif
  No se encontraron definiciones de hardware validas en la carpeta espressif.
```

**¿Cómo lo solucionamos?**. Pues después de buscar, la solución que he aplicado es la de mover el directorio **esp32** al directorio **espressif**. Ambas carpetas se generan en el proceso de instalación anterior.  
![Selecci-n_148](/content/images/2018/05/Selecci-n_148.png)

Ahora si!!!. Cuando vamos a *Herramientas* -> *Placa ""* podemos ver listado de placas ESP32:  
![Selecci-n_150](/content/images/2018/05/Selecci-n_150.png)

Ahora vamos a buscar el típico ejemplo del "nodo emisor" y "nodo receptor" de paquetes, de tal forma podremos verificar que todo está funcionado. Para ello utilizaremos ejemplos y librería LoRa que podremos encontrar en este repositorio GitHub: <https://github.com/osresearch/esp32-ttgo> y dentro del directorio *libraries* veremos una llamada **LoRa.zip**:

![Selecci-n_142](/content/images/2018/05/Selecci-n_142.png)

![Selecci-n_155](/content/images/2018/05/Selecci-n_155.png)

Es una librería que podemos cargar desde el IDE de Arduino, por el menú de *Programa* -> *Incluir Librería* -> *Añadir mediante ZIP*.

Una vez tengamos cargada la librería, podemos ir a los ejemplos de LoRa que nos aparecen en el menú *Archivo* -> *Ejemplos* -> *LoRa*. Primero vamos a programar el nodo receptor. Pero...

## Problema 2. *esptool.py line 34 in import serial*

![Selecci-n_151](/content/images/2018/05/Selecci-n_151.png)

Este problema traducido, quiere decir que el script de **esptool.py** no encuentra el módulo/librería de Python *serial*.

**¿Cómo lo corregimos?**. La solución es sencilla, demos instalar el módulo *pyserial*, para ello desde la consola ejecutamos (previamente tienes que tener la herramienta **pip** instalada):

*sudo pip install pyserial*

![Selecci-n_152](/content/images/2018/05/Selecci-n_152.png)

Una vez corregido este par de errores ya podemos compilar y transferir sin problema nuestro código a nodo receptor.

En la consola de Arduino veremos todo el proceso de compilación y flasheo del módulo.

![Selecci-n_156](/content/images/2018/05/Selecci-n_156.png)

Si monitorizamos el *Serial* podemos ver cómo responde nuestro *nodo receptor*.

![Selecci-n_157](/content/images/2018/05/Selecci-n_157.png)

Tan solo queda hacer lo mismo con el *nodo emisor*, pero cargando el código ejemplo opuesto ;). Y veremos cómo nuestro *nodo receptor* comienza a recibir los paquetes.

![](/content/images/2018/05/IMG_yp47mb.jpg)

![Selecci-n_159](/content/images/2018/05/Selecci-n_159.png)

![](/content/images/2018/05/IMG_ncsvyq.jpg)

Hasta aquí el "hola mundo" de LoRa con dos nodos conectados punto a punto.

Aquí tenéis una guía muy completa y fantástica de LoRa con The Things Networks del equipo de Bricolabs:  
<https://bricolabs.cc/wiki/guias/lora_ttn>
