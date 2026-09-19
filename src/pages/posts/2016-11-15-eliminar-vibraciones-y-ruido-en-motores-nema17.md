---
layout: ../../layouts/post.astro
title: "Eliminar vibraciones y ruido en motores NEMA17"
pubDate: 2016-11-15
description: "Típico comentario de cuñao (futuro ingeniero que trabaja en robótica): \"Y esto hace tanto ruido???\"...pues la verdad, es que \"la excusa\" al"
author: "akirasan"
isPinned: false
excerpt: "Típico comentario de cuñao (futuro ingeniero que trabaja en robótica): \"Y esto hace tanto ruido???\"...pues la verdad, es que \"la excusa\" al"
image:
  src: "/content/images/2016/11/IMG_20161115_215534-2.jpg"
  alt: "Eliminar vibraciones y ruido en motores NEMA17"
tags: []
---

Típico comentario de cuñao (futuro ingeniero que trabaja en robótica): "Y esto hace tanto ruido???"...pues la verdad, es que "la excusa" al típico ruido que hacía mi impresora Delta, la había atribuido a las vibraciones que transmiten de los tres motores que trabajan al mismo tiempo, a la estructura de la impresora por cada una de las torres. Me parecía normal, hasta este comentario "del cuñao".

Así que me puse a buscar una solución por Internet. Y encontré los "dampers", o lo que es lo mismo, unos amortiguadores diseñados para los motores NEMA17. Se colocan entre la estructura y el motor. Puedes encontrar unos hechos del típico corcho. Yo directamente los he descartado y he comprado este **pack de tres en eBay por unos 15€** (gastos incluidos). Son fáciles de encontrar con un búsqueda que diga "dampers nema17":

![](/content/images/2016/11/IMG_20161111_181508-1.jpg)

Me ha costado un poco instalarlos, no porque sea difícil (todo lo contrario) sino porque hace que **los motores se separen unos 6mm** de donde están instalados:

![](/content/images/2016/11/IMG_20161114_205543_20161114_222051.jpg)

Esos 6mm de espacio que se desplazan los tres motores que están en la base he tenido que recablear, reubicar la fuente de alimentación y la electrónica,... pero ha merecido la pena como ya veréis en un video al final.

Ya veis el poco espacio que tenía en la base de la impresora Delta.  
![](/content/images/2016/11/IMG_20161113_112226_20161114_064404.jpg)

Ahora, entre la estructura y el motor queda el damper, el cual reducirá la vibración que genera el motor y se traspasa a la estructura directamente.  
![](/content/images/2016/11/pixlr_20161114144538350.jpg)

![](/content/images/2016/11/IMG_20161112_115527_20161114_064405-1.jpg)

Puede ser que para otro tipo de impresoras (como cartesianas), el resultado no sea muy evidente, ya que depende de la construcción. Pero en una Delta de este tipo, donde los tres motores están en constante movimiento en cada capa de impresión el resultado es notorio.

He grabado un vídeo con el típico *antes y después*. No tiene una calidad de audio excelente, pero he intentado grabarlo bajo las mismas condiciones para comparar el resultado.

Si buscáis por YouTube encontraréis vídeos con mejor comparativa, como por ejemplo el que podéis ver [en este link](https://www.youtube.com/watch?v=h98P44xqVwg)
