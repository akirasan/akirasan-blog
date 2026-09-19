---
layout: ../../layouts/post.astro
title: "Copy-Paste de un objeto real"
pubDate: 2017-07-03
description: "La impresión 3D tiene ese poder mágico de generar objetos físicos a partir de información digital no tangible. Pero, ¿cómo hacemos para hace"
author: "akirasan"
isPinned: false
excerpt: "La impresión 3D tiene ese poder mágico de generar objetos físicos a partir de información digital no tangible. Pero, ¿cómo hacemos para hace"
image:
  src: "/content/images/2017/07/IMG_20170703_181251-1.jpg"
  alt: "Copy-Paste de un objeto real"
tags: []
---

La impresión 3D tiene ese poder mágico de generar objetos físicos a partir de información digital no tangible. Pero, ¿cómo hacemos para hacer el sentido inverso, de objeto físico a digital?. Bueno, el concepto de **digitalizar un objeto en 3D** mediante scanners, es *muy sencillo* ;)...aquí vamos a ver como hacerlo **sin scanner 3D**.

Todo empezó una tarde de verano y unas patatas fritas. Si, así vi ese pequeño objeto hecho de plástico que me servía para pinchar y coger patatas,...y me pregunté ¿plástico?¿impresora3D?,...¿lo podría replicar!!!?. Así comenzó este reto personal (vamos una chorrada).

![](/content/images/2017/07/IMG_20170701_205000.jpg)

Desde Inkscape, vamos a calcar el modelo. Para ello, lo primero que hice fue una foto con un fondo que me permita tener referencia de escala.

![](/content/images/2017/07/Selecci-n_040.png)

Dentro del Inkscape vamos a tener que activar una rejilla a escala de mm igual que vemos en la foto. Aquí hay que ir jugando un poco incluso la imagen, hasta alinearlo correctamente:

![](/content/images/2017/07/Selecci-n_047.png)

Ahora, mediante utilización del lápiz de curvas de Bézier y rectas, vamos repasando los bordes, mas o menos,...luego acabaremos ajustándolo.

![](/content/images/2017/07/Selecci-n_041.png)

Ajustamos lo máximo a la silueta del objeto

![](/content/images/2017/07/Selecci-n_042.png)

Y ahora, pensando en 3D, tenemos que tener en cuenta, que este objeto tiene un vacío central también un poco peculiar, una forma que tendremos que calcar ya que luego nos servirá para realizar el vaciado.

![](/content/images/2017/07/Selecci-n_043.png)

Bien, pues ya tenemos nuestro diseño calcado en 2D

![](/content/images/2017/07/Selecci-n_044.png)

Siguiente paso, lo guardamos como SVG y nos vamos a FreeCAD para importarlo en un nuevo diseño.

![](/content/images/2017/07/Selecci-n_048.png)

Cómo se puede ver tenemos los dos paths diferenciados, eso nos permitirá extruir cada uno de ellos.

![](/content/images/2017/07/Selecci-n_045.png)

Luego desplazamos ligeramente en Z el objeto extruido del interior y se lo restamos a la forma sólida del tenedor para conseguir hacer el vaciado por dentro:

![](/content/images/2017/07/Selecci-n_046.png)

Listo!!! ya tenemos nuestro paso del mundo físico al mundo digital en 3D. Ahora ya solo falta hacer las pruebas de impresión y tal vez ajustar algo la escala, ya que a veces no es 100% exacto e implica modificar rescalando desde Inkscape el dibujo 2D vectorizado.

![](/content/images/2017/07/IMG_20170702_102335.jpg)  
![](/content/images/2017/07/IMG_20170702_102254.jpg)

Y por último, después de hacer las primeras pruebas de impresión, le añadimos un toque mas personal

![](/content/images/2017/07/IMG_20170703_181251.jpg)  
![](/content/images/2017/07/IMG_20170703_181335.jpg)

El fuente en FreeCAD y el fichero STL lo podéis descargar de mi [GitHub](https://github.com/akirasan/3dprinting) o desde [Thingiverse](https://www.thingiverse.com/thing:2416806).
