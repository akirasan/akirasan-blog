---
layout: ../../layouts/post.astro
title: "Bleak Theme para Ghost"
pubDate: 2016-07-04
description: "Acabo de instalar y activar el tema  para el blog (). Me parece que se ajusta bastante mas al contenido del blog, ya que trata mejor los apa"
author: "akirasan"
isPinned: false
excerpt: "Acabo de instalar y activar el tema  para el blog (). Me parece que se ajusta bastante mas al contenido del blog, ya que trata mejor los apa"
image:
  src: "/content/images/2016/07/bleak_theme_demo.jpg"
  alt: "Bleak Theme para Ghost"
tags: ["Ghost"]
---

Acabo de instalar y activar el tema [Bleak](https://github.com/zutrinken/bleak) para el blog ([aquí una demo](http://bleak.zutrinken.com/)). Me parece que se ajusta bastante mas al contenido del blog, ya que trata mejor los apartados de código, tiene por defecto botones para compartir en redes sociales y tiene un aspecto similar a [Saga](http://saga.gustavlindqvist.se/) en la página principal. Además sigue manteniendo la línea simplista y minimalista que quiero dar al blog.

Aprovecho para explicar la instalación: cómo el tema lo podemos descargar de un repositorio GitHub, podemos hacer lo siguiente desde el directorio raíz donde tengamos nuestro Ghost instalado:

* Descargar en */content/themes* el tema Bleak
* Cambiar el usuario propietario (hemos descargado con *root*, en mi caso uso *ghost*
* Reiniciar el Ghost

```auto
sudo git https://github.com/zutrinken/bleak.git content/themes/bleak
sudo chown -R ghost:ghost content/themes/bleak
sudo service ghost restart
```
