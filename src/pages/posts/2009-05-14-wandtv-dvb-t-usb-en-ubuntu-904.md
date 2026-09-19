---
layout: ../../layouts/post.astro
title: "WandTV DVB-T USB en Ubuntu 9.04"
pubDate: 2009-05-14
description: "Hoy me ha llegado una de mis compras en , se trata de un sintonizador USB de TDT, es el . Para poder utilizarlo en Ubuntu hay que seguir los"
author: "akirasan"
isPinned: false
excerpt: "Hoy me ha llegado una de mis compras en , se trata de un sintonizador USB de TDT, es el . Para poder utilizarlo en Ubuntu hay que seguir los"
image:
  src: ""
  alt: "WandTV DVB-T USB en Ubuntu 9.04"
tags: []
---

Hoy me ha llegado una de mis compras en [DealExtreme](http://www.dealextreme.com/ "http://www.dealextreme.com/"), se trata de un sintonizador USB de TDT, es el [WandTV DVB-T](http://www.dealextreme.com/details.dx/sku.8325 "http://www.dealextreme.com/details.dx/sku.8325"). Para poder utilizarlo en Ubuntu hay que seguir los típicos pasos en linux: bajarse los fuentes, compilarlos e instalarlos, fácil?? no??,...jejeje. Esto es normal porque el chipset **EC168** que lleva el WandTV está soportado bajo el proyecto [LinuxTV](http://www.linuxtv.org/ "http://www.linuxtv.org/").

Bueno al lio, para hacer que funcione hay que seguir estos pasos:

> (entrar en modo root, yo he ejecutado todos los pasos como root)
>
> ```auto
> sudo -s
> ```
>
> ```auto
> apt-get install mercurial
> ```
>
> ...bajarse el fuente:
>
> ```auto
> hg clone http://linuxtv.org/hg/~anttip/ec168/
> ```
>
> ...compilamos:
>
> ```auto
> make
> ```
>
> ...instalamos:
>
> ```auto
> make install
> ```

Ahora hay que bajarse el firmware desde aquí <http://palosaari.fi/linux/v4l-dvb/firmware/ec168/dvb-usb-ec168.fw> copiar el fichero **dvd-usb-ec168.fw** en el directorio **/lib/firmware/**

y listo!!! a disfrutar de la TDT,...por cierto yo he utilizado el Kaffeine como reproductor, aunque es para KDE en Gnome funciona perfectamente.

*UPDATE: aquí está la wiki de LinuxTV para mas información:* [*http://linuxtv.org/wiki/index.php/How\_to\_Obtain,\_Build\_and\_Install\_V4L-DVB\_Device\_Drivers*](http://linuxtv.org/wiki/index.php/How_to_Obtain,_Build_and_Install_V4L-DVB_Device_Drivers)

Fuente [Foro DealExtreme](https://www.dealextreme.com/forums/Default.dx/sku.8325~threadid.278942 "https://www.dealextreme.com/forums/Default.dx/sku.8325~threadid.278942")
