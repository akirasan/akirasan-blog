---
layout: ../../layouts/post.astro
title: "Lanzar aplicación Python tras iniciar RaspberryPi"
pubDate: 2018-10-02
description: "Realizando un proyecto conjunto con Raul () en , nos hemos encontrado con una duda que puede ser muy común en proyectos de este tipo."
author: "akirasan"
isPinned: false
excerpt: "Realizando un proyecto conjunto con Raul () en , nos hemos encontrado con una duda que puede ser muy común en proyectos de este tipo."
image:
  src: "/content/images/2018/10/IMG_20181002_073733_Bokeh-5.jpg"
  alt: "Lanzar aplicación Python tras iniciar RaspberryPi"
tags: ["python", "Linux", "ubuntu", "RaspberryPi", "commandline"]
---

Realizando un proyecto conjunto con Raul ([@raulrubioescoda](https://twitter.com/raulrubioescoda)) en [Ripolab Hacklab](https://ripolab.org), nos hemos encontrado con una duda que puede ser muy común en proyectos de este tipo.

#### El proyecto PhotoBooth

El proyecto consiste en un fotomaton portátil para fiestas o eventos, el cual sube las fotos que se toman a un servicio web o las comparte en redes sociales.

Hemos utilizado una RaspberryPi con una PiCamera para tomar las fotos.

#### El requisito de inicio desatendido

Es evidente que lo que queremos es automatizar al máximo el proceso desatendido a la hora de encenderse el PhotoBooth. Por lo tanto, queremos que nuestra aplicación, desarrollado en Python, se lance de forma automática, sin tener que lanzar de manualmente la aplicación.

#### Cómo ejecutar automáticamente una aplicación gráfica en Python tras un reinio del sistema

**En *cron* tenemos una directiva llamada *@reboot*** la cual nos permite ejecutar un comando o script, una vez, cuando el sistema se inicia.

Por otro lado al ser una aplicación gráfica, tenemos que derivar su salida hacia el escritorio gráfico. Para ello ya sabemos que podemos utilizar un *export DISPLAY=:0.0* dónde normalmente se suele tener el escritorio principal.

Pues con estas dos informaciones ya podemos construir un script bash, el cual vamos a programar en cron para que se ejecute tras un startup del sistema.

Creamos un fichero, por ejemplo *app\_python.sh*, con las siguientes lineas:

`#!/bin/bash`  
`export DISPLAY=":0.0"`  
`python3 /home/pi/photobooth/photobooth_v2.py`

Y le damos permisos de ejecución al script, mediante un *chmod 777* (cualquier usuario podrá ejecutar y modificar el script, revisa las restricciones que necesites).

Ahora vamos a programar nuestro cron, para ello editamos la tabla de cron:

`crontab -e`

Y añadimos la siguiente línea, para nuestro ejemplo:

`@reboot /home/akirasan/app_python.sh`

Y ya lo tenemos todo, ahora solo falta probarlo!!!. Cuando el sistema se inicie, y aparezca el escritorio gráfico, se ejecutará nuestra app Python.

A veces puede tardar un poco, pero es normal, hay que esperer unos segundos.
