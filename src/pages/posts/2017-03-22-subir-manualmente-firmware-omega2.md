---
layout: ../../layouts/post.astro
title: "Subir manualmente firmware Omega2+"
pubDate: 2017-03-22
description: "Que no, que no,...que esto no se acaba, a ver si os pensáis que todo ha sido coser y cantar. Pues no,...en un momento, no se cuando, he deja"
author: "akirasan"
isPinned: false
excerpt: "Que no, que no,...que esto no se acaba, a ver si os pensáis que todo ha sido coser y cantar. Pues no,...en un momento, no se cuando, he deja"
image:
  src: "/content/images/2017/03/IMG_20170310_162808-1.jpg"
  alt: "Subir manualmente firmware Omega2+"
tags: ["onion", "IoT", "Linux"]
---

Que no, que no,...que esto no se acaba, a ver si os pensáis que todo ha sido coser y cantar. Pues no,...en un momento, no se cuando, he dejado a la pobre Omega2+ frita porque he sido tan listo de hacerle un reset de fábrica desde la Power Dock (que se puede hacer dejando pulsado el botón de reset durante 10 segundos). Así que he tenido que restaurar desde una tarjeta microSD.

Pero antes, dejadme que os enseñe lo que te sale cuando restauras de fábrica e intentas acceder a la dirección de antes: <http://192.168.3.1>:

###### Index of /console/

![](/content/images/2017/03/Selecci-n_016.png)

El procedimiento para instalarle el firmware es sencillo:

1. Nos bajamos el firmware del [repositorio de imágenes](http://repo.onion.io/omega2/images/) de Onion.io. Aquí vigilad porque para la Omega2+, el nombre que tiene es *omega2p-version.bin*
2. Guardamos el fichero *.bin* en una tarjeta microSD y la insertamos en la Omega2+.
3. Arrancamos nuestra Omega2+
4. Cómo siempre emitirá el punto de acceso Wifi, nos conectamos y esta vez usamos SSH, ya que el entorno web no funciona.
5. Buscamos donde está montada la tarjeta microSD y vamos hasta que vemos el fichero firmware que hemos grabado.
6. Ejecutamos el comando: `sysupgrade -n fichero_firmware.bin`

![](/content/images/2017/03/Selecci-n_017.png)

Esperamos pacientemente y tras el boot todo estará como cuando la compramos :)

Un apunte que me ha hecho reflexionar y me parece interesante dejarlo claro: con la opción "-n" en el `sysupgrade` **eliminas toda configuración anterior, si no quieres perderla, no la uses**. Mi instalación del firmware ha sido por manazas y no por querer actualizar la versión. Gracias [Gustavo @Yespiros](https://twitter.com/Yespiros) por tu comentario en Twitter.

![](/content/images/2017/03/IMG_20170310_162808.jpg)  
![](/content/images/2017/03/IMG_20170310_162717.jpg)

Si por alguna razón estos pasos no te funcionan [aquí te dejo el enlace a la documentación Onion.io](https://docs.onion.io/omega2-docs/manual-firmware-installation.html) para la instalación manual del firmware.
