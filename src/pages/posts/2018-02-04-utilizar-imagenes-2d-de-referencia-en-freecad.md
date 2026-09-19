---
layout: ../../layouts/post.astro
title: "Utilizar imágenes 2D de referencia en FreeCAD"
pubDate: 2018-02-04
description: "A veces cuando diseñamos una pieza en 3D en , va bien tener una imagen real de algún objeto con el que vamos a trabajar. El problema, es que"
author: "akirasan"
isPinned: false
excerpt: "A veces cuando diseñamos una pieza en 3D en , va bien tener una imagen real de algún objeto con el que vamos a trabajar. El problema, es que"
image:
  src: "/content/images/2018/02/upload-1517761106.jpg"
  alt: "Utilizar imágenes 2D de referencia en FreeCAD"
tags: ["freecad", "3dprinting"]
---

A veces cuando diseñamos una pieza en 3D en [FreeCAD](https://www.freecadweb.org/), va bien tener una imagen real de algún objeto con el que vamos a trabajar. El problema, es que utilizar una imagen de referencia no nos sirve para trabajar con la escala correcta del objeto. Pero esto tiene solución mediante la macro [***imageScaleReference***](https://github.com/jonnor/projects/blob/master/freecad/macros/), que se encuentra en el GitHub de <https://github.com/jonnor>

**OJO!!!** Hay una macro similar que se instala desde el *Addon Manager* de FreeCAD, pero hay que tener encuenta que tiene un fallo en el código [***Macro Image Scaling***](https://www.freecadweb.org/wiki/Macro_Image_Scaling) Que además **tiene un error de código**, el foro de *freecadweb.org* [aquí podéis ver el código corregido](https://forum.freecadweb.org/viewtopic.php?f=22&t=13877) (se puede corregir fácilmente con la edición de Macros)

## Instalación de la Macro

La instalación de la macro *Image Scale Reference* es relativamente sencilla. Nos bajamos la macro [***imageScaleReference***](https://github.com/jonnor/projects/blob/master/freecad/macros/imageScaleReference.FCMacro) desde el GitHub y guardamos el fichero como **.FCMacro** en el directorio propio de macros del usuario:

* Para Linux estará en */home/<usuario>/.FreeCAD/Macro/*
* Para Windows tenéis el [How to install macros](https://www.freecadweb.org/wiki/How_to_install_macros) en la wiki de FreeCAD.

![Selecci-n_152](/content/images/2018/02/Selecci-n_152.jpg)

Una vez dejamos éste fichero en el directorio de macros, ya lo tenemos disponible directamente para utilizar.

## Vamos a reescalar una imagen

Ahora que tenemos la macro instalada, desde la sección ***Image*** vamos a cargar la imagen de referencia mediante el icono de *cargar*.

![Selecci-n_131](/content/images/2018/02/Selecci-n_131.jpg)

Seleccionamos el plano de refencia donde cargaremos la imagen. En mi caso será *Plano XY* porque la foto está tomada desde arriba.

![Selecci-n_132](/content/images/2018/02/Selecci-n_132.jpg)

Bien, ahora toca hacer la primera medición que nos servirá de referencia a la hora de hacer la corrección y poder escalar la imagen a las dimensiones correctas.

Seleccionamos dos puntos de la imagen. Si os fijáis en este ejemplo, la distancia entre esos puntos es de 713,33mm (según la escala actual de la imagen). Lo que haremos será medir la distancia real que hay entre esos dos puntos.

![Selecci-n_133](/content/images/2018/02/Selecci-n_133.jpg)

![](/content/images/2018/02/upload-1517760641.jpg)

Ahora que tenemos la distancia real, vamos a ajustar la imagen de la escala correcta utilizano la macro. Seleccionamos los objetos, **imagen y distancia**, y nos vamos al menú de *Macros*.

![Selecci-n_134](/content/images/2018/02/Selecci-n_134.jpg)  
![Selecci-n_135-1](/content/images/2018/02/Selecci-n_135-1.jpg)

Buscamos la macro **imageScaleReference** y la ejecutamos. Lo que tenemos que hacer es muy sencillo, simplemente cambiar al valor real que hemos medido. En este ejemplo pasamos/ajustamos **713,3mm --> 79,5mm**

![Selecci-n_136](/content/images/2018/02/Selecci-n_136.jpg)

![Selecci-n_137](/content/images/2018/02/Selecci-n_137.jpg)

![Selecci-n_139](/content/images/2018/02/Selecci-n_139.jpg)

Ya tenemos la imagen prácticamente ajustada a una escala real. Si ahora medimos alguna parte de la imagen, veremos está prácticamente al objeto real. Cuanto mas ajustadas la medidas, mejor aproximación.

![](/content/images/2018/02/upload-1517760624.jpg)

## ¿Qué pasa con la Macro Image Scaling?

Cómo he comentado al inicio del artículo, la macro que nos proporciona FreeCAD por defecto tiene un error en el código y es necesario bajar el código adecuado (os he puesto el link al inicio).

![Selecci-n_129](/content/images/2018/02/Selecci-n_129.jpg)

En la pestaña de **Macros** bucamos en el listado de macros disponibles **Image Scaling**, y le damos a *Install/Upgrade*

![Selecci-n_130](/content/images/2018/02/Selecci-n_130.jpg)

Una vez tenemos corregido el código (que puede ser tan secillo como bajarse el código y sobreescribir la macro que ha instalado el *Addon Manager* en el directorio de instalación), hay que tener en cuenta que esta macro funciona ligeramente diferente a la macro anterior.

Partiendo de una imagen, ejecutamos la macro (sin hacer ninguna medición previa). Nos solicitará los dos puntos de referencia, los cuales usará para ajustar la escala de la imagen.

Una vez introducimos estos dos puntos, indicaremos la distancia *en el mundo real* de estos dos puntos. Y listo!!! ya estaría ajustada.
