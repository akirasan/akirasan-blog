---
layout: ../../layouts/post.astro
title: "Resetear Unity a la configuración inicial"
pubDate: 2011-06-17
description: "Si eres como yo que no paras de tocar el Unity para adaptarlo a tus necesidades,...y ya lo tocas tanto que montas un sigral. Solo tienes que"
author: "akirasan"
isPinned: false
excerpt: "Si eres como yo que no paras de tocar el Unity para adaptarlo a tus necesidades,...y ya lo tocas tanto que montas un sigral. Solo tienes que"
image:
  src: ""
  alt: "Resetear Unity a la configuración inicial"
tags: []
---

Si eres como yo que no paras de tocar el Unity para adaptarlo a tus necesidades,...y ya lo tocas tanto que montas un sigral. Solo tienes que ejecutar el siguiente comando en un terminal para resetear a la configuración inicial de Unity.

> ```auto
> unity --reset
> ```

También si quieres eliminar los iconos añadidos:
> ```auto
> unity --reset-icons
> ```

...y bueno si estás como loco con el Compiz y te ha pasado lo mismo, ejecuta esto para eliminar tus cambios:
> ```auto
> gconftool-2 --recursive-unset /apps/compiz-1
> ```
