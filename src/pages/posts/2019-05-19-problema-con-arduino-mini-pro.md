---
layout: ../../layouts/post.astro
title: "Problema con Arduino Mini Pro y Serial"
pubDate: 2019-05-19
description: "Me he vuelto loco con este tema tema, así que me creo esta entrada tanto para mi, cómo para a alguien que le pase lo mismo."
author: "akirasan"
isPinned: false
excerpt: "Me he vuelto loco con este tema tema, así que me creo esta entrada tanto para mi, cómo para a alguien que le pase lo mismo."
image:
  src: "/content/images/2019/05/arduinominipro.png"
  alt: "Problema con Arduino Mini Pro y Serial"
tags: ["Arduino"]
---

Me he vuelto loco con este tema tema, así que me creo esta entrada tanto para mi, cómo para a alguien que le pase lo mismo.

Intento programar un **Arduino Mini Pro de 3v3 8Mhz**, mediante un adaptador FTDI. El proceso funciona, pero cualquier cosa que intento ver por el Serial da errores, **esos caracteres raros** que aparecen cuando no has indicado la velocidad de puerto serie con la velocidad que has definido en la programación, y con la misma velocidad en baudios.

![](/content/images/2019/05/2019-05-19_23-39.png)![](/content/images/2019/05/error_mini_pro_arduino_serial.png)

No se entiende, ¿verdad?. Pues después de varias pruebas y revisar y tal,...[he encontrado en un foro la solución](https://forum.arduino.cc/index.php/topic,46458.0.html), y pasa por cambiar la velocidad del Serial a una superior:

![](/content/images/2019/05/error_mini_pro_arduino_serial_2.png)

Pues nada, ahí lo dejo...
