---
layout: ../../layouts/post.astro
title: "Reparar ficheros MKV que no se reproducen en el cliente Plex de Samsung"
pubDate: 2013-07-16
description: "Si te has encontrado con el siguiente problema: no poder reproducir algunos ficheros MKV en el cliente Plex de una smartTV de Samsung (o pos"
author: "akirasan"
isPinned: false
excerpt: "Si te has encontrado con el siguiente problema: no poder reproducir algunos ficheros MKV en el cliente Plex de una smartTV de Samsung (o pos"
image:
  src: ""
  alt: "Reparar ficheros MKV que no se reproducen en el cliente Plex de Samsung"
tags: []
---

Si te has encontrado con el siguiente problema: no poder reproducir algunos ficheros MKV en el cliente Plex de una smartTV de Samsung (o posiblemente en otro escenario también falle), aquí he encontrado una solución que a mi me ha funcionado muy bien. Normalmente el fallo es que al querer reproducir el fichero, comienza a cargar y luego no reproduce. Esto es debido a que el fichero MKV tiene que ser reparado. Para ello existe un script muy sencillo que permite corregir ese defecto.

Para que el script funcione es necesario que tengamos instalado el paquete mkvtoolnix.

```auto
apt-get -s install mkvtoolnix
```

Ahora hace falta crear el script (copiando el código) o bajandolo de esta dirección: <https://www.dropbox.com/s/xcfpqjb6hf1zqsa/repairMKV.zip>

La utilización del script es sencilla, simplemente hay que pasar el nombre del fichero mkv como parámetro y esperar a que el proceso finalice. Cuando esté acabado, prueba a reproducir nuevamente el fichero y listo!!!,...ya funciona!!!

Fuente: <http://forums.plexapp.com/index.php/topic/63691-how-to-automated-linux-script-for-fixing-broken-mkv-files-works-with-sickbeard-too/>
