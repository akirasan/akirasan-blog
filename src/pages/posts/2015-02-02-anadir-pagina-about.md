---
layout: ../../layouts/post.astro
title: "Añadir página \"About\""
pubDate: 2015-02-02
description: "Si el tema que utilizas en el blog Ghost no tiene soporte para una página *\"About\"* dondes expliques de que va tu blog, puedes utilizar esta"
author: "akirasan"
isPinned: false
excerpt: "Si el tema que utilizas en el blog Ghost no tiene soporte para una página *\"About\"* dondes expliques de que va tu blog, puedes utilizar esta"
image:
  src: ""
  alt: "Añadir página \"About\""
tags: ["Ghost"]
---

Si el tema que utilizas en el blog Ghost no tiene soporte para una página *"About"* dondes expliques de que va tu blog, puedes utilizar esta opción (tema de referencia utilizado: [Casper](http://allghostthemes.com/casper/)).

Lo primero es crear una página estática, para ello generamos una entrada *"NEW POST"* pero marcamos la opción de *""*

![](/content/images/2015/02/page_about.JPG)

Luego modificamos el fichero `index.hbs` que encontraremos en `<path_ghost>/content/themes/casper/`, añadiendo la siguiente línea al lado del link de *subscripción* (el **/about/** es el nombre de nuestra página estática que hemos creado anteriormente, así que si le hemos puesto otro nombre tenedlo en cuenta):

`<a class="subscribe-button icon-ghost" href="{{@blog.url}}/about/">About</a>`

El código final quedaría algo como esto:  
![](/content/images/2015/02/script_about.JPG)  
El resultado lo podéis ver en este mismo blog, donde aparece un botón nuevo en la página principal.  
![](/content/images/2015/02/boton_about.JPG)
