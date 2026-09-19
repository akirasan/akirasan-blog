---
layout: ../../layouts/post.astro
title: "JPEGmini vs jpegoptim"
pubDate: 2011-09-01
description: "Vía , descubro este nuevo servicio web, que premite aligerar el peso de nuestras fotos sin perder prácticamente la calidad de la imagen. El"
author: "akirasan"
isPinned: false
excerpt: "Vía , descubro este nuevo servicio web, que premite aligerar el peso de nuestras fotos sin perder prácticamente la calidad de la imagen. El"
image:
  src: ""
  alt: "JPEGmini vs jpegoptim"
tags: []
---

Vía [Xataka Foto](http://www.xatakafoto.com/actualidad/jpegmini-maxima-compresion-sin-perder-calidad "http://www.xatakafoto.com/actualidad/jpegmini-maxima-compresion-sin-perder-calidad"), descubro este nuevo servicio web, que premite aligerar el peso de nuestras fotos sin perder prácticamente la calidad de la imagen. El servicio ([JPEGmini](http://www.jpegmini.com/main/home "jpegmini")) funciona de la siguiente forma: subes a tu espacio (previo registro) las fotos que quieras convertir, una vez subidas el sistema las procesa y te envía un correo para que en 9 días te descargues las fotos optimizadas en peso. La cosa está bien,...no??. Bueno pues como yo utilizo el [jpegoptim](http://freshmeat.net/projects/jpegoptim/ "http://freshmeat.net/projects/jpegoptim/") desde hace tiempo y normalmente reduzco a un 80 de calidad (ya me parece muy buen el resultado), he estado haciendo pruebas para comparar ***JPEGmini*** con ***jpegoptim***, y la verdad es que me quedo con jpegoptim: sencillo, en linea de comandos, rápido y no tengo que enviar las fotos ni subirlas previamente a ningún sitio,...

**Foto original (tamaño 4,2Mb)**:

[![DSC_2694_original_OK](http://static.zooomr.com/images/10064387_0c41aa676a.jpg)](http://www.zooomr.com/photos/akirasan/10064387/ "Photo Sharing")

**Foto tratada con JPEGmini (tamaño 670Kb):**

[![DSC_2694_mini](http://static.zooomr.com/images/10064382_19c54987a3.jpg)](http://www.zooomr.com/photos/akirasan/10064382/ "Photo Sharing")

**Foto tratada con jpegoptim parámetro -m80 (tamaño 704Kb):**

[![DSC_2694_80](http://static.zooomr.com/images/10064385_60234fd31e.jpg)](http://www.zooomr.com/photos/akirasan/10064385/ "Photo Sharing")

Un ***crop al 100%*** de la misma zona (original, jpegmini, jpegoptim):

[![DSC_2694_original](http://static.zooomr.com/images/10064381_7d38cc401e.jpg)](http://www.zooomr.com/photos/akirasan/10064381/ "Photo Sharing")

[![DSC_2694_mini_crop](http://static.zooomr.com/images/10064386_4077d2d623.jpg)](http://www.zooomr.com/photos/akirasan/10064386/ "Photo Sharing")

[![DSC_2694_op80](http://static.zooomr.com/images/10064384_cd53078e28.jpg)](http://www.zooomr.com/photos/akirasan/10064384/ "Photo Sharing")

...y para comparar un poco mas, unos crop al 100% con parámetros a -m75 (**tamaño 559Kb**) y -m70 (**tamaño 484Kb**):

[![DSC_2694_op75](http://static.zooomr.com/images/10064396_a0deb26315.jpg)](http://www.zooomr.com/photos/akirasan/10064396/ "Photo Sharing")

[![DSC_2694_op70](http://static.zooomr.com/images/10064394_0d2105f941.jpg)](http://www.zooomr.com/photos/akirasan/10064394/ "Photo Sharing")

**Resultado**: el JPEGmini es una kk de servicio. Lo que tienen que hacer la gente es no fliparse tanto y reducir con alguna aplicación en su PC el tamaño antes de compartir las fotos por la web.

Mis pruebas me llevan a la conclusión que JPEGmini utiliza un ratio de compresión similar al parámetro "-mXX" de jpgoptim, de entre un valor 70-80. <http://www.akirasan.net/?p=716>
