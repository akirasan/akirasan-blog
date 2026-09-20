---
layout: ../../layouts/post.astro
title: "Nuevo blog en Astro: cambiando de aires (y de dominio)"
pubDate: 2026-09-19
description: "Toca cambiar de aires. Adiós a akirasan.net y a Ghost para pasar a akirasan.xyz con Astro, GitHub y Cloudflare."
author: "akirasan"
isPinned: false
excerpt: "Toca cambiar de aires. Adiós a akirasan.net y a Ghost para pasar a akirasan.xyz con Astro, GitHub y Cloudflare."
image:
  src: "/images/newblogastro.jpg"
  alt: "Nuevo blog en Astro akirasan.xyz"
tags: ["Astro", "maker", "DIY", "Cloudflare", "GitHub"]
---

Toca cambiar de aires, y esta vez obligados.

Si seguías mis publicaciones por aquí sabrás que el blog ha estado funcionando durante años en el dominio `akirasan.net`. Pero por un despiste con el registrador acabé perdiendo el dominio. Una faena después de tanto tiempo, pero bueno, tampoco hay que lamentarse: he aprovechado la ocasión para hacer borrón y cuenta nueva y mudarnos a **[akirasan.xyz](https://akirasan.xyz)**.

Y ya puestos en faena, tocaba cambio total de tecnología.

![](/images/newblogastro.jpg)

### De Ghost en local a Astro

Hasta ahora el blog lo tenía montado en [Ghost](https://ghost.org/) corriendo en un servidor en local en casa. Para escribir va genial, pero al final dependes de una máquina encendida, mantener Node, bases de datos, copias de seguridad caseras y cruzar los dedos para que la conexión de casa no dé guerra. 

Como sabéis que me gusta simplificar y cacharrear, he preferido pasar a una solución estática y mucho más ágil:

* **[Astro](https://astro.build/):** Todo el blog corre sobre ficheros Markdown. He cogido un tema base y lo he modificado a mi gusto para darle ese aire de terminal y notas de taller que buscaba.
* **[GitHub](https://github.com/):** Todos los posts, imágenes y el código residen directamente en un repositorio de Git. Escribir un post es tan simple como abrir un fichero `.md` y hacer commit.
* **[Cloudflare Pages](https://pages.cloudflare.com/):** Gestiona el dominio y el despliegue automático. En cuanto subo un post a GitHub, Cloudflare compila la página en segundos y la sirve volando. Sin servidores locales ni historias de mantenimiento.

### Modo maker ON

Al final, quitarme el engorro de mantener el servidor local me deja más tiempo libre para lo que realmente tiene que ser este blog: un cuaderno de notas abierto con mis proyectos de taller, diseño de PCBs en KiCAD, cacharreo con el ESP8266, sensores, impresión 3D y sistemas Linux. Proyectos que nacen de una idea en una servilleta y acaban funcionando entre cables y scripts ;)

Espero que os guste el lavado de cara. ¡Seguimos cacharreando!