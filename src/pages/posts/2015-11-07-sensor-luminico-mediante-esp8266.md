---
layout: ../../layouts/post.astro
title: "Sensor lumínico mediante ESP8266"
pubDate: 2015-11-07
description: "En el mundo del **Internet of Things** se quiere medir y controlar todo. En ese afán había pensado en crear un sensor lumínico que conectado"
author: "akirasan"
isPinned: false
excerpt: "En el mundo del **Internet of Things** se quiere medir y controlar todo. En ese afán había pensado en crear un sensor lumínico que conectado"
image:
  src: "/content/images/2015/11/IMG_20151107_231251.jpg"
  alt: "Sensor lumínico mediante ESP8266"
tags: ["IoT", "ESP8266"]
---

En el mundo del **Internet of Things** se quiere medir y controlar todo. En ese afán había pensado en crear un sensor lumínico que conectado al ya conocido módulo wifi ESP8266 permitiera, por ejemplo, saber si nos hemos dejado la luz encendida de una habitación.

Para ello había pensado en leer el valor del voltaje que obtendría de un divisor de voltaje donde una de las resistencias fuera una fotoresitencia, es decir, que varía su valor ohms en función de la cantidad de luz que recibe.

Ya había comenzado a prototipar y para realizar las primeras pruebas, cuando a la hora de leer el valor he descubierto que el puerto GPIO el módulo ESP8266 es digital y no analógico :/ así que no puedo medir valores de voltaje intermedios de una forma directa.

![](/content/images/2015/11/IMG_20151107_231254.jpg#small)  
Habrá que buscar alguna alternativa que no sea muy compleja. Lo primero y mas evidente que me viene a la cabeza es utilizar un arduino.
