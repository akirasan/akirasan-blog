---
layout: ../../layouts/post.astro
title: "jpegoptim: Reducir tamaño de las fotos desde consola"
pubDate: 2011-01-22
description: "En alguna ocasión te encuentras en la necesidad de compartir fotos con alguien y claro, con las cámaras actuales de chorrocientes Megapixels"
author: "akirasan"
isPinned: false
excerpt: "En alguna ocasión te encuentras en la necesidad de compartir fotos con alguien y claro, con las cámaras actuales de chorrocientes Megapixels"
image:
  src: ""
  alt: "jpegoptim: Reducir tamaño de las fotos desde consola"
tags: []
---

En alguna ocasión te encuentras en la necesidad de compartir fotos con alguien y claro, con las cámaras actuales de chorrocientes Megapixels las fotos ocupan de 6Mb para arriba. Por eso una herramienta que nos puede simplificar el tema es esta: **jpegoptim**. Se instala desde los paquetes oficiales de Ubuntu y se ejecuta desde consola. Yo la utilizo con estas opciones:

> ```auto
> jpegoptim -m90 -ptv *.JPG
> ```

Opciones:

**-m90**: indica la calidad maxima, en este caso un 90%
**-p**: mantiene la hora y fecha del fichero original
**-t**: información del total
**-v**: nos muestra información del proceso

De esta forma **se consigue comprimir hasta un 70%** el tamaño de las fotos y la verdad es que se agradece a la hora de enviarlas por mail,...
