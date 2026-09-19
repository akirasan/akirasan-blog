---
layout: ../../layouts/post.astro
title: "La badge de Ripolab Hacklab"
pubDate: 2018-05-24
description: "La badge de  **ya es una realidad**."
author: "akirasan"
isPinned: false
excerpt: "La badge de  **ya es una realidad**."
image:
  src: "/content/images/2018/05/IMG_20180524_224703.jpg"
  alt: "La badge de Ripolab Hacklab"
tags: ["ESP8266", "DIY", "PCB", "KiCAD", "Ripolab"]
---

La badge de [Ripolab Hacklab](http://ripolab.org) **ya es una realidad**.

La idea surgió viendo el escenario actual de badge que se estaban realizando para eventos. Primero me llamó, la ya conocida badge de [OSHWI](https://github.com/hulkco/oshwi_2017) creada por [Gustavo Reynaga](https://twitter.com/gsreynaga) para la [OSHWDem 2017](https://oshwdem.org/2017/12/resumen-oshwdem-2017/). El [TindieBlinky badge kits](https://twitter.com/tindie/status/997509159376441345) que me pareció supersencillo. Y luego estuve colaborando en un par de badges, entre ellas la que ha salido a la luz recientemente, la de [Granabot 2018](https://github.com/fgcoca/Granabot-badge).  
Junto con las ganas de aprender a diseñar PCBs más complejas con KiCAD ([mi primera PCB fue muy sencilla](http://akirasan.net/baliza-vial-sos/)), pues me tiré a la piscina para diseñar una PCB con un módulo ESP8266 y varios LED’s WS2812B, con el logo y forma de Ripolab Hacklab.

### Primeros pasos

Primero y para empezar poco a poco, hice el diseño de una breadboard o placa de prototipado. La idea era probar básicamente el diseño y validar que las “antenitas” del logo no quedaran débiles y se rompieran. Pero la sorpresa fue que quedó fantásticamente bien!!!

![](/content/images/2018/05/IMG_20180223_142816.jpg)

![](/content/images/2018/05/IMG_20180226_012319.jpg)

### Diseño del esquemático

El diseño del esquema está basado en el que realizó Gustavo Reynaga para la OSHWI. Únicamente realicé algunos cambios: incluir un micro interruptor para no tener que desconectar la batería cada vez, el doble de LED’s, eliminé los conectores THT y solo algunos los pasé a SMD para poder programar el ESP8266 una vez soldado a la placa y el modelo de ESP8266-12E. Éste último cambio hizo modificar el footprint y cometí un error en diseño que explicaré mas adelante.

![Selecci-n_232](/content/images/2018/05/Selecci-n_232.png)

### Primera versión: FALLO!!!

Si. A pesar de haber revisado varias veces el diseño, cometí varios errores que los puede comprobar en el primer pedido a PCBWay:

* El footprint de ESP8266-12E no era correcto. La relación de pines estaba totalmente desplazada. La librería que había utilizado, no estaba actualizada correctamente, así que lo modifiqué.
* La capa de GND no llegaba a tres de los LED’s.

Para descartar mas fallos, monté y corregí todos los errores que había cometido y funcionó.

![](/content/images/2018/05/IMG_20180425_204240.jpg)

El error en el diseño, me permitió ver algunas cosas que no me gustaron, así que rediseñé nuevamente y corregí los errores.

![](/content/images/2018/05/IMG_20180524_224549.jpg)

### Versión final

Finalmente la badge es una realidad. Una vez soldados todos los componentes y programado el ESP8266 todo funciona correctamente. Aquí os dejo un vídeo con un ejemplo básico de la librería Adafruit para controlar los LEDs WS2812B.

Las PCB’s las he solicitado a [PCBWay](https://www.pcbway.com/). Me han ofrecido una buena relación calidad-precio, aparte de un servicio muy bueno en cuanto a comunicación y envío. Por el momento no tengo ninguna queja con ellos.

Si os gusta la PCB podéis votar el proyecto en [PCBWay](https://www.pcbway.com/project/shareproject/Ripolab_Hacklab_Badge.html).

Y si queréis toda la información, esquemáticos, la BOM, diseño PCB, etc,…lo podéis encontrar tanto en [mi repositorio Github](https://github.com/akirasan/Placas-PCB/tree/master/ripolab_pcb) cómo en el de [Ripolab Hacklab](https://github.com/ripolab/Placas-PCB/tree/master/ripolab_pcb)
