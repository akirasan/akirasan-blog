---
layout: ../../layouts/post.astro
title: "Bed Skew Compensation"
pubDate: 2021-02-07
description: "Si tus piezas 3D no salen a escuadra, tal vez tengas un problema de construcción y si no lo puedes solucionar, tal vez mediante software y l"
author: "akirasan"
isPinned: false
excerpt: "Si tus piezas 3D no salen a escuadra, tal vez tengas un problema de construcción y si no lo puedes solucionar, tal vez mediante software y l"
image:
  src: "/content/images/2021/02/bedskew_marlin_portada.png"
  alt: "Bed Skew Compensation"
tags: []
---

Si tus piezas 3D no salen a escuadra, tal vez tengas un problema de construcción y si no lo puedes solucionar, tal vez mediante software y la opción de Bed Skew Compensation que trae Marlin, lo puedas solucionar.

Las impresoras 3D DIY a veces tienen "complicaciones" en su construcción, ángulos descompensados, piezas impresas que llevan milímetros de error de otras impresoras, etc...En fin, pequeños errores que se suman y suman, y hacen que la impresión de nuestra impresora no quede del todo bien.

Marlin tiene una gran opción llamada **Bed Skew Compensation**, una opción que nos ayuda a compensar la desviación producida por el hardware no ajustado.

![](/content/images/2021/02/bedskew_marlin.png)

¿Cómo funciona? ¿cómo se configura? en el vídeo de Chris Riley, que os dejo por aquí, lo explica muy bien <https://www.youtube.com/watch?v=YfAb5IaHDSo>. Pero os dejo un ejemplo de lo resultados que he obtenido yo configurando esta opción.

## Configurando Bed Skew Compensation

Lo primero que tenemos que hacer es imprimir el modelo que está en **Thingiverse** (<https://www.thingiverse.com/thing:2563185>) sin la opción activada en el firmware de Marlin.

![](/content/images/2021/02/image.png)

Y ahora analizamos el resultado (vamos a ajustar ejes XY), tenemos que hacer mediciones de las diagonales y el valor obtenido multiplicarlo x2, y ese será el valor que utilizaremos en la configuración de Marlin:

![](/content/images/2021/02/IMG_20210207_123128.jpg)![](/content/images/2021/02/image-1.png)

Se ve claramente que no está a escuadra!!!

![](/content/images/2021/02/IMG_20210207_122652-1.jpg)![](/content/images/2021/02/image-4.png)

Con un medidor de ángulos podemos ver el error de 0.8º, parece poco, pero os digo que a simple vista se puede ver.

![](/content/images/2021/02/IMG_20210207_122723.jpg)

Una vez configurado Marlin, subimos el firmware y volvemos a imprimir el mismo modelo y analizamos el resultado,...sorprendente!!!

![](/content/images/2021/02/IMG_20210207_122540.jpg)

La pieza de la izquierda tiene el error de 0.8º y la de la derecha está corregida mediante software. Realmente un gran resultado.

![](/content/images/2021/02/IMG_20210207_123545-1.jpg)

Este ajuste también se puede hacer con los ejes XZ y YZ, imprimiendo el otro modelo y ajustando los parámetros que medimos de las diagonales:

![](/content/images/2021/02/image-5.png)
