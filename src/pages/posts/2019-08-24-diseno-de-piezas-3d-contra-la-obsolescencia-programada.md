---
layout: ../../layouts/post.astro
title: "Diseño de piezas 3D contra la obsolescencia programada"
pubDate: 2019-08-24
description: "Se habla mucho sobre la  de los productos que solemos comprar. Por otro lado se quiere potenciar la , es decir, evitar al máximo los residuo"
author: "akirasan"
isPinned: false
excerpt: "Se habla mucho sobre la  de los productos que solemos comprar. Por otro lado se quiere potenciar la , es decir, evitar al máximo los residuo"
image:
  src: "/content/images/2019/08/portada.png"
  alt: "Diseño de piezas 3D contra la obsolescencia programada"
tags: ["blender", "3D", "3dprinting", "DIY", "maker"]
---

Se habla mucho sobre la [obsolescencia programada](https://es.wikipedia.org/wiki/Obsolescencia_programada) de los productos que solemos comprar. Por otro lado se quiere potenciar la [economía circular](https://es.wikipedia.org/wiki/Econom%C3%ADa_circular), es decir, evitar al máximo los residuos que generamos, reciclar o reutilizar de otra forma aquellos objetos/productos que han llegado a su vida útil.

El poder que tiene la impresión 3D sobre estas palancas solo acaba de comenzar. Personalmente creo que un futuro la impresión 3D nos ayudará a autoreparar aquellos objetos que tiraríamos a la basura simplemente porque se ha roto una parte, ya que nos cuesta encontrar recambios o el precio que tiene es tan absurdo que no merece la pena.

En fin este artículo no va orientado a dar lecciones de conciencia, es un artículo para todos aquellos que quieren intentar reparar, mediante impresión 3D, aquellos objetos que se rompen.

Voy a explicar el *workflow* que sigo yo a la hora de replicar una pieza rota y que se puede replicar mediante impresión 3D. El *workflow* no es extrictamente fijo, en ocasiones se puede alterar, pero creo que puede ser una buena forma de iniciar el proceso. Lo que está claro es que **mis herramientas son 100% opensource** y básicamente son: **FreeCAD**, **Inkscape**, **Blender** y **Slic3r**.

## El proyecto - maneta cierre trasporting

El proyecto nació cómo necesidad: se ha roto una de las manetas de cierre de un trasporting (por cierto no tengo animales domésticos).

### Paso 1. Analizar la complejidad de la pieza.

Lo primero es darle vueltas a la pieza y analizar su forma. Es un paso clave, ya que nos permitirá saber cómo replicar la  pieza. Éste paso a medida que vayamos cogiendo experiencia será mas fácil.

![](/content/images/2019/08/pieza_sujeta.jpg)

No hay que preocuparse mucho, si la estratégia que pensamos no es del todo correcta, de los errores también se aprende y tal vez nos implique hacer varias iteraciones hasta encontrar la forma correcta.

![](/content/images/2019/08/2019-08-24_16-49.png)

Para esta pieza en concreto he decidido diseñar la sección y extruirla. Luego hacer los detalles que tiene, que en este caso son dos cortes en una zona.

### Paso 2. Calcar en 2D para crear una sección.

Para hacer la sección de la pieza voy a calcar la foto que he hecho en Inkscape, de esta forma puedo conseguir darle las formas curvadas tan particulares que tiene esta pieza.

![](/content/images/2019/08/calcar-pieza-inkscape_3.png)![](/content/images/2019/08/calcar-pieza-inkscape.png)

Vamos a seguir el contorno utilizando [curvas de Bézier](https://es.wikipedia.org/wiki/Curva_de_B%C3%A9zier), ajustando todo lo que podamos a su detalle.

Por ahora no nos vamos a preocupar de sus dimensiones reales, únicamente de copiar su contorno lo más exacto posible.

Ahora una vez realizado esto, vamos a generar la imagen vectorial SVG que cargaremos posteriormente en Blender para hacer la extrusión. Pero antes es recomendable tener en cuenta dos cosas:

1. **El contorno** que hemos creado tiene que estar **100% cerrado**.
2. **Aplicaremos un modificador** de Inkscape llamado **Aplanar Béziers**, que no ayudará a tener un vectorial mucho mas sencillo de interpretar por otras herramientas.

![](/content/images/2019/08/calcar-pieza-inkscape_2.png)

Una vez realizado esto, grabamos nuestro diseño como [SVG](https://es.wikipedia.org/wiki/Gr%C3%A1ficos_vectoriales_escalables).

### Paso 3. Modelar en 3D

Llega el momento de darle volumen y forma tridimensional a nuestra pieza. Para ello importaremos el diseño SVG en Blender (en este caso, es Blender, pero podría ser FreeCAD o similar).

![](/content/images/2019/08/blender_1.png)

Cuando importamos un fichero vectorial en Blender no es un objeto en forma de malla, sino que es un objeto compuesto por un grupo de curvas, por lo tanto tendremos que convertirlo a *Mesh* para poder seguir trabajando con él:

![](/content/images/2019/08/blender_3.png)

Ahora que es un objeto malla *Mesh*, podemos seleccionarlo y entrando **en modo edición** *Edit Mode* (en Blender es seleccionando el objeto y pulsando la tecla *TAB*), **seleccionamos todos los vértices y generamos una cara** (en Blender para seleccionar todos los vértices pulsamos la tecla A (de *All*), y para generar la cara, la tecla F (de *Face*)).

![](/content/images/2019/08/blender_4.png)

Ahora que ya tenemos una cara, la podemos extruir en altura (en Blender se hace pulsando la tecla X (de *eXtrude*)). Aquí comenzaremos ya a tener en cuenta las dimensiones que necesitamos.

![](/content/images/2019/08/blender_5.png)

Tiene buena pinta!!!, al menos en forma y diseño se parece a la original. Pero falta incluir un par de detalles, dos cortes necesarios para que encaje la pieza.

Esas dos hendiduras las he realizado con el modificador boleano *Boolean*, haciendo una resta con dos cubos que he añadido.

![](/content/images/2019/08/blender_6.png)

Ya tenemos nuestra pieza prácticamente lista para ser enviada a la impresión 3D, ajustada en dimensiones y con los *detallitos* necesarios. Pero **en ocasiones me he encontrado con problemas a la hora de laminar** una pieza realizada en Blender, así que trato de **aplicar este paso antes de exportar a STL** y pasar a Slic3r: **Triangulizar caras**

![](/content/images/2019/08/blender_7.png)

### Paso 4. Imprimir en 3D

No voy a entrar en detalle de este último paso, ya que la idea del artículo es explicar el workflow de diseño. Solo comentar que **las pruebas de impresión no sean un calvario y tener que esperar horas** para verificar si se ajusta bien, en estos casos en los que la pieza es ha diseñado como una sección, **se puede imprimir solo una parte**, unos 10mm de esa sección y verificar si se ajusta a la sección real. **Aplicando los cambios y correcciones de dimensiones** en el diseño 3D, que sean necesarias.

Finalmente obtendremos una pieza muy similar a la original y que nos permitirá seguir utilizando nuestro objeto reparado.

![](/content/images/2019/08/IMG_20190812_184603.jpg)

Ya podemos utilizar de nuevo nuestro transporting!!!

![](/content/images/2019/08/IMG_20190812_184549.jpg)

### Compartir!!!

Los diseños cómo siempre los comparto, así que los tenéis disponibles en: [Github](https://github.com/akirasan/3dprinting/tree/master/Pieza%20cierre%20transporting) o [Thingiverse](https://www.thingiverse.com/thing:3826909/files)

## Otros ejemplos

En ocasiones las piezas no van a ser tan sencillas cómo las de este ejemplo (je,je,je), pero será cuestión de buscar la mejor estrategia de modelado y hacer mas iteraciones. Es cuestión de conseguir práctica y soltura, nada mas.

Aquí otro ejemplo, curiosamente otra pieza para otro transporting (aquí hay negocio!!!). Una pieza mas compleja, no solo en el diseño sino también a la hora de imprimirla.

![](/content/images/2019/08/2019-08-24_16-57.png)![](/content/images/2019/08/render.png)![](/content/images/2019/08/cierre2.jpg)![](/content/images/2019/08/cierre1.jpg)
