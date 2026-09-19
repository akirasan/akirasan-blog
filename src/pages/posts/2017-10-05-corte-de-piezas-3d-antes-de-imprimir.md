---
layout: ../../layouts/post.astro
title: "Corte de piezas 3D antes de imprimir"
pubDate: 2017-10-05
description: "Imagínate que tienes que imprimir una pieza en tu impresora 3D y ooppss!!! no cabe!!!, es demasiado grande. Bueno pues hay una solución medi"
author: "akirasan"
isPinned: false
excerpt: "Imagínate que tienes que imprimir una pieza en tu impresora 3D y ooppss!!! no cabe!!!, es demasiado grande. Bueno pues hay una solución medi"
image:
  src: "/content/images/2017/10/Selecci-n_091.png"
  alt: "Corte de piezas 3D antes de imprimir"
tags: []
---

Imagínate que tienes que imprimir una pieza en tu impresora 3D y ooppss!!! no cabe!!!, es demasiado grande. Bueno pues hay una solución mediante Slic3r.  
![Selecci-n_087](/content/images/2017/09/Selecci-n_087.png)

Seleccionamos el objeto y la en el menú superior encontramos la opción **Cut**  
![Selecci-n_088](/content/images/2017/09/Selecci-n_088.png)

Ahora podemos jugar con estos parámetros:

* **Axis**: Sobre que eje queremos realizar el corte (X, Y o Z).
* **Slice**: Desplazamos el eje de corte, ahí definimos la capa sobre la que cortar.
* **Upper part**: Lo marcaremos si queremos conservar un lado del corte.
* **Lower part**: Lo marcaremos si queremos conservar la otra parte del corte.  
  ![Selecci-n_090](/content/images/2017/09/Selecci-n_090.png)  
  ![Selecci-n_091](/content/images/2017/09/Selecci-n_091.png)

Cuando ya hemos jugado y definido por donde cortar la pieza, pulsamos **Perform cut**  
![Selecci-n_089](/content/images/2017/09/Selecci-n_089.png)

También podemos utilizar la opción **Cut by grid**, la cual va a realizar un corte en cuadrículas. Indicaremos el tamaño de las secciones mediante valores a X e Y, y automáticamente nos va a generar trocitos de esas dimensiones:  
![Selecci-n_092](/content/images/2017/09/Selecci-n_092.png)  
![Selecci-n_093](/content/images/2017/09/Selecci-n_093.png)

Y aquí el Hulk troceado con piezas de máximo 100x100.  
![Selecci-n_094](/content/images/2017/09/Selecci-n_094.png)

**IMPORTANTE** Una vez tenemos una pieza cortada en trozos, es importante que exportemos cada una de las piezas a un fichero STL nuevo, de esta forma no perderemos las secciones que posteriormente iremos imprimiendo.  
![Selecci-n_096](/content/images/2017/09/Selecci-n_096.png)

Este paso es importante ya que realizar el corte exactamente igual para que luego las piezas encajen puede ser bastante complicado.

Para este ejemplo tal vez no tenga mucho sentido, pero imaginad una pieza como ésta: se trata de algo tan grande,...muy grande, en la imagen la base de la impresora es la gris!!!.  
![IMG_20170908_220759](/content/images/2017/10/IMG_20170908_220759.jpg)

Lo seccionamos en partes mediante *Cut by grid* y toma!!!, todo segmentado  
![IMG_20170908_220957](/content/images/2017/10/IMG_20170908_220957.jpg)

Bueno **¿y una vez impresas las piezas que?**, pues os voy a enseñar un caso práctico de una máquina de arcade que tiene piezas muy grandes. Las tuve que seccionar y para reforzar un poco diseñé unas *grapas* pequeñas:  
![IMG_20170830_164916](/content/images/2017/10/IMG_20170830_164916.jpg)  
![IMG_20170830_165033](/content/images/2017/10/IMG_20170830_165033.jpg)  
![IMG_20170830_165046](/content/images/2017/10/IMG_20170830_165046.jpg)

Para unirlas y que queden fuertes, he utilizado una pega especial con el nombre comercial Pattex:  
![IMG_20170830_174611](/content/images/2017/10/IMG_20170830_174611.jpg)  
![IMG_20170830_174618](/content/images/2017/10/IMG_20170830_174618.jpg)  
![IMG_20170830_174712](/content/images/2017/10/IMG_20170830_174712.jpg)

Evidentemente tenemos que repasar y eliminar todo el sobrante y dejar secar.

![IMG_20170830_175146](/content/images/2017/10/IMG_20170830_175146.jpg)  
![IMG_20170830_175214](/content/images/2017/10/IMG_20170830_175214.jpg)

Luego una vez seca, se puede lijar o volver a repasar si es necesario. Aquí otra pieza impresa en cuatro partes. Aunque parezca fea, luego va pintada y queda bastante bien.  
![IMG_20170917_142440](/content/images/2017/10/IMG_20170917_142440.jpg)
