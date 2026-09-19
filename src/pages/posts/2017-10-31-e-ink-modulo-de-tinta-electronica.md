---
layout: ../../layouts/post.astro
title: "Round 1: E-Ink módulo de tinta electrónica"
pubDate: 2017-10-31
description: "He titulado esta entrada como \"*round 1*\" porque esto aún no ha acabado...Quiero decir, que por el momento y para quienes están empezando a"
author: "akirasan"
isPinned: false
excerpt: "He titulado esta entrada como \"*round 1*\" porque esto aún no ha acabado...Quiero decir, que por el momento y para quienes están empezando a"
image:
  src: ""
  alt: "Round 1: E-Ink módulo de tinta electrónica"
tags: []
---

He titulado esta entrada como "*round 1*" porque esto aún no ha acabado...Quiero decir, que por el momento y para quienes están empezando a interesar por éste módulo de tinta electrónica pueda encontrar un forma de probarlo mediante el ejemplo que da el propio fabricante [Waveshare.com](https://www.waveshare.com/product/modules/oleds-lcds/e-paper/2.9inch-e-paper-module-b.htm)

Vamos al lío: se trata de un módulo de pantalla E-Ink, de 2.9 pulgadas, resolución de 296x128, con controlador incorporado, que se comunica a través de la interfaz SPI.

La gente de [Waveshare ha creado una Wiki](https://www.waveshare.com/wiki/2.9inch_e-Paper_Module_(B)) para poder consultar toda la info de los módulos y podamos conectarlos a una Raspberri Pi, STM32 y **Arduino**, que es la que nos interesa.

### Cableado de conexión con Arduino

Lo primero que podemos hacer es cablear la pantalla con nuestro Arduino. Para ello debemos tener encuenta el pinout de Arduino referente a la interface SPI. Normalmente "casi todos" vamos a utilizar el mismo pinout de Arduino, pero revisadlo por si utilizais algún Arduino diferente. Yo lo he probado con Arduino UNO y un Arduino Mini Pro:

![arduino_eink29_pinout-1](/content/images/2017/10/arduino_eink29_pinout-1.jpg)

Nos bajamos de la Wiki el paquete <https://www.waveshare.com/wiki/File:2.9inch_e-paper_module_b_code.7z> y lo descomprimimos. Dentro veremos que hay tres carpetas (*arduino*, *raspberrypi* y *stm32*), una por cada solución soportada. A nosotros nos interesa ir por Arduino.

El código de Arduino tiene un par de errores que puedes corregir tu mismo y que se encuentra en los ficheros ***epdif.cpp*** y ***epdif.h***:

![epdif.cpp](/content/images/2017/10/epdif.cpp.jpg)

![epdif.h](/content/images/2017/10/epdif.h.jpg)

Ahora que tenemos corregido el código, podemos crear un ZIP y cargarlo como una librería mas en el IDE de Arduino ([aquí os dejo una reparada](https://github.com/akirasan/eink2.9-waveshare/blob/master/epd2in9_waveshare/libraries/epd2in9_fixed.zip).

![zip_libreria](/content/images/2017/10/zip_libreria.jpg)

Para empezar se puede utilizar el fichero *.ino* que viene de ejemplo-demo: **epd2in9-demo.ino**

He creado un repo en Github con lo que vaya avanzando sobre ésta pantalla e-ink: <https://github.com/akirasan/eink2.9-waveshare>
