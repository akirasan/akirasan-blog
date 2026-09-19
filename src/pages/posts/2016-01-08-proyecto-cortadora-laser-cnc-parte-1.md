---
layout: ../../layouts/post.astro
title: "Proyecto: Cortadora láser CNC (parte 1)"
pubDate: 2016-01-08
description: "Viendo la idea de una cortadora láser reutilizando material viejo, como dos unidades de DVD y Arduino, me he puesto manos a la obra a invest"
author: "akirasan"
isPinned: false
excerpt: "Viendo la idea de una cortadora láser reutilizando material viejo, como dos unidades de DVD y Arduino, me he puesto manos a la obra a invest"
image:
  src: "/content/images/2016/01/IMG_20151227_211011.jpg"
  alt: "Proyecto: Cortadora láser CNC (parte 1)"
tags: ["DIY", "Arduino"]
---

Viendo la idea de una cortadora láser reutilizando material viejo, como dos unidades de DVD y Arduino, me he puesto manos a la obra a investigar sobre el tema. La idea surgió de un tweet que me llevó a este blog de [Davide Gironi](http://davidegironi.blogspot.it/2014/07/38mm-x-38mm-laser-engraver-build-using.html#.VpAvZnXhBxh) donde saqué parte de la información

He de decir que yo de este tema tenía: idea igual a cero, vamos que ni idea de lo que era CNC ni de como controlar este tipo de dispositivos.

Para que los que tampoco tengáis ni idea como yo, hoy a poneros algo de teoría básica para entenderlo todo mejor:

[CNC](https://es.wikipedia.org/wiki/Control_num%C3%A9rico): El **CNC** es el acrónimo de Control Numérico Computazional, y se viene utilizando desde los años 50. Un sistema basado en comandos de texto para el control de motores que ha ido evolucionando de tal forma que permite un control exacto mediante un sistema cartesiano. Por ejemplo, algunos de los códigos que se utilizan son:

* G00: El trayecto programado se realiza a la máxima velocidad posible, es decir, a la velocidad de desplazamiento en rápido.
* G01: Los ejes se gobiernan de tal forma que la herramienta se mueve a lo largo de una línea recta.
* G02: Interpolación circular en sentido horario.
* G03: Interpolación circular en sentido antihorario.

[grbl](https://github.com/grbl/grbl): **grbl** el software/driver/programa que se *carga* sobre Arduino para realizar el control de los motores. Recibe los comandos GCODE por USB y mueve los motores en consecuencia. Es capaz de controlar hasta tres motores, lo que sería un eje de coordenadas tridimensional: X, y, z. Nosotros únicamente vamos a utilizar x, y. La librería grbl una vez cargada sobre Arduino, tiene una serie de pineado específico y fijo que tendremos que conocer y utilizar, aquí os dejo:

![](/content/images/2016/01/Grbl_Pin_Diagram.png#small)

Podéis encontrar las últimas actualizaciones de los pinouts según versión de la librería grbl en su [wiki](https://github.com/grbl/grbl/wiki/Connecting-Grbl)

[bCNC](https://github.com/vlachoudis/bCNC): El bCNC es el software que se conectará al puerto serie de nuestro Arduino, en el que la librería grbl estará escuchando los comandos GCODE que vamos a enviar y moverá los motores de paso en función a la configuración que le definamos. De este tipo de software hay varios, pero este me ha resultado interesante porque es *cross platform program* (Windows, Linux, Mac) y está escrito en Python. Pero evidentemente aquí cada uno es libre de utilizar el que le parezca.

![bCNC](/content/images/2016/01/bCNC.png#small)

Básicamente lo que haremos con este programa será calibrar los dos ejes (x, y) y leer el fichero tipo *.ngc* que contendrá los comandos CNC para que la librería **grbl** lo interprete.

Hasta aquí esta primera parte, os dejo algunas imágenes iniciales del despiece de las unidades de DVD. OJO!!!, no tiréis nada!!!, nunca se sabe cuando se podrían reutilizar las piezas para otro proyecto.

![](/content/images/2016/01/IMG_20151220_195627.jpg#small)  
![](/content/images/2016/01/IMG_20151220_202321.jpg#small)  
![](/content/images/2016/01/IMG_20151227_154044.jpg#small)  
![](/content/images/2016/01/IMG_20151227_154405.jpg#small)  
![](/content/images/2016/01/IMG_20151227_202701.jpg#small)  
![](/content/images/2016/01/IMG_20151227_203854.jpg#small)
