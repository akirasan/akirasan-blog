---
layout: ../../layouts/post.astro
title: "Trabajar con medidas en Blender"
pubDate: 2018-11-09
description: "Os voy a explicar cómo configurar Blender para trabajar con medidas y poder diseñar piezas lo más exactas posibles. Normalmente me gusta tra"
author: "akirasan"
isPinned: false
excerpt: "Os voy a explicar cómo configurar Blender para trabajar con medidas y poder diseñar piezas lo más exactas posibles. Normalmente me gusta tra"
image:
  src: "/content/images/2018/11/Selecci-n_468.png"
  alt: "Trabajar con medidas en Blender"
tags: ["blender", "3D", "3dprinting"]
---

Os voy a explicar cómo configurar Blender para trabajar con medidas y poder diseñar piezas lo más exactas posibles. Normalmente me gusta trabajar con FreeCAD para el diseño de piezas, ya que su potencia como entorno paramétrico es genial para hacer correcciones rápidas y ajustes a las piezas.

La pregunta me vino por el Twitter de César Fernández [@CaesarFdez](https://twitter.com/CaesarFdez/status/1060777572651417601), así que he preferido explicarlo en pequeño post.

Lo primero que vamos a configurar en nuestro proyecto va a ser la escena. Para ello seleccionamos el icono de ***Scene*** y en la sección de ***Units*** (Unidades) usamos el desplegable para seleccionar si queremos trabajar en centímetros, metros, mm, pies, pulgadas,..vamos a seleccionar milímetros.

![](/content/images/2018/11/2018-11-09_20-53.png)

scene blender

Automáticamente se nos actualiza el campo ***Length*** y ***Angle***, donde nos indica que trabajaremos en el sistema métrico y grados. El campo ***Unit Scale*** nos sirve para indicar la escala o resolución decimal, por defecto 0,001mm ya me parece bastante bien.

![](/content/images/2018/11/2018-11-09_20-54-1.png)

Ahora seleccionando nuestro objeto y pulsando la **tecla N** o desplegando el menú oculto, veremos las propiedades de nuestro objeto

![](/content/images/2018/11/2018-11-09_20-58-1.png)

En el apartado de ***Dimensiones*** veremos que por defecto el cubo que nos aparece al iniciar Blender tiene unas dimensiones de 2mm x 2mm x 2mm

![](/content/images/2018/11/2018-11-09_21-00-2.png)

Evidentemente desde este apartado se pueden modificar manualmente las dimensiones del objeto, así podemos fijar medidas exactas.

Cuando tenemos una figura y queremos comprobar las dimensiones que tiene, podemos activar las opciones de ***Edge Info*** y ***Face Info*** que se encuentran en el menú de propiedades del objeto, pero en el **Modo Edición**, no en el **Modo Objeto**: Seleccionamos el objeto y cambiamos a Modo Edición pulsando la **tecla** **TAB** o desde el menú inferior.

![](/content/images/2018/11/Selecci-n_462.png)

Al entrar en el Modo Edición vamos al menú de la derecha y buscamos la sección ***Mesh Display*** y dentro de esa sección tenemos *Edge Info* y *Face Info*.

![](/content/images/2018/11/Selecci-n_464.png)

Si por ejemplo seleccionamos una cara de nuestro objeto y marcamos la opción de *Length*, nos mostrará la longitud de las aristas. Cómo nuestro cubo tiene una dimensiones de 2mm veremos un 2 sobre cada arista.

![](/content/images/2018/11/Selecci-n_465.png)

Si activamos mas opciones podremos tener mas información, como el ángulo y el área, tanto de las aristas como de las caras.

![](/content/images/2018/11/Selecci-n_466.png)

Y por último, una cosa a tener en cuenta: cómo para ver la información de dimensiones, trabajamos en modo edición, solo veremos esta información para aquellos objetos en las que activemos.

![](/content/images/2018/11/Selecci-n_467.png)
