---
layout: ../../layouts/post.astro
title: "ATtiny85 y leds RGB WS2812"
pubDate: 2019-06-15
description: "Si quieres utilizar un ATtiny85 con leds RGB WS2812b y tienes problemas para que funcione, te cuento cómo lo he solucionado yo."
author: "akirasan"
isPinned: false
excerpt: "Si quieres utilizar un ATtiny85 con leds RGB WS2812b y tienes problemas para que funcione, te cuento cómo lo he solucionado yo."
image:
  src: "/content/images/2019/06/photo_2019-06-15_21-10-06.jpg"
  alt: "ATtiny85 y leds RGB WS2812"
tags: []
---

Si quieres utilizar un ATtiny85 con leds RGB WS2812b y tienes problemas para que funcione, te cuento cómo lo he solucionado yo.

Preparas un prototipo con un ATtiny85 y tres leds WS2812b para hacer un semáforo (RED ALERT!!!), utilizando la librería [Adafruit\_NeoPixel](https://github.com/adafruit/Adafruit_NeoPixel) y el IDE de Arduino. Subes el sketch mediante un Arduino UNO preparado como ISP (ya sabes, primero tienes que programar el Arduino con el sketch de ejemplo *ArduinoISP*),…y te llevas un auténtico chasco porque los leds no hacen lo que tu esperas. Simplemente se quedan encendidos, fijos, con un color blanco.

A partir de aquí comienzas una odisea revisando el cableado, el pin de datos, etc,…todo bien, pero no funciona. Pues esto es lo que me ha pasado a mí, así que te explico la solución:

Normalmente los **ATtiny85 vienen prefijados a una frecuencia de reloj de 1Mhz**. Nosotros necesitamos que funcione con el reloj interno a 16Mhz (opción que seleccionamos en el IDE de Arduino a la hora de seleccionar la placa y cómo está preparada la librería Neopixel de Adrafruit), por lo tanto hay que tener en cuenta que: **cuando se utiliza un chip nuevo o cambiamos la velocidad del reloj**, se debe seleccionar previamente la opción de **Burn Bootloader** (Quemar Bootloader). Esto establecerá a qué velocidad de reloj debe funcionar nuestro chip.

![](/content/images/2019/06/2019-06-15_21-06.png)

Una vez ya hemos establecido la frecuencia en la que tiene que trabajar, ya podemos subir nuestro programa. Verás como funciona correctamente ;)

Datasheet ATtiny85: <http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-2586-AVR-8-bit-Microcontroller-ATtiny25-ATtiny45-ATtiny85_Datasheet.pdf>
