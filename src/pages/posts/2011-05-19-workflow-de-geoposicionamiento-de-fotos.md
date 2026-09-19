---
layout: ../../layouts/post.astro
title: "Workflow de geoposicionamiento de fotos"
pubDate: 2011-05-19
description: "A título informativo, sin entrar en muchos detalles, voy a explicar el workflow (pasos) que sigo para geoposicionar las fotos en linux (en m"
author: "akirasan"
isPinned: false
excerpt: "A título informativo, sin entrar en muchos detalles, voy a explicar el workflow (pasos) que sigo para geoposicionar las fotos en linux (en m"
image:
  src: ""
  alt: "Workflow de geoposicionamiento de fotos"
tags: []
---

A título informativo, sin entrar en muchos detalles, voy a explicar el workflow (pasos) que sigo para geoposicionar las fotos en linux (en mi caso con Ubuntu 11.04).

* **Hardware**: [Holux M-241](http://www.holux.com/JCore/en/products/products_content.jsp?pno=341 "http://www.holux.com/JCore/en/products/products_content.jsp?pno=341"), [Nikon D90](http://www.nikon.es/es_ES/product/digital-cameras/slr/consumer/d90 "http://www.nikon.es/es_ES/product/digital-cameras/slr/consumer/d90")
* **Software**: [mtkbabel](http://mtkbabel.sourceforge.net/ "http://mtkbabel.sourceforge.net/") (instalable desde repositorio), [gpxsplitter.py](http://blog.samat.org/2011/02/16/gpxsplitter-Split-GPX-files-with-their-waypoints "http://blog.samat.org/2011/02/16/gpxsplitter-Split-GPX-files-with-their-waypoints"), [Geotag](http://geotag.sourceforge.net/?q=node/3 "http://geotag.sourceforge.net/?q=node/3") (webstart en java, requiere instalación de las Exiftool (desde repositorio como libimage-exiftool-perl), [jhead](http://packages.ubuntu.com/hardy/jhead "http://packages.ubuntu.com/hardy/jhead") (instalable desde repositorio).

1. Copiamos todas las fotos en un directorio temporal.
2. Abrimos un terminal y vamos al directorio temporal.
3. Descargamos la información del GPS en formato *.gpx* con ***mtkbabel***:


   ```auto
   mtkbabel -s 38400 -f datos_tmp -t
   ```
4. (opcional) particionamos el fichero .gpx en varios con información diaria:


   ```auto
   gpxsplitter.py datos_tmp_trk.gpx
   ```
5. **(recomendación)** Estos dos pasos anteriores los podéis encapsular en un script en bash para simplificar el trabajo.
6. Arrancamos el ***Geotag*** y seleccionamos las fotos a geoposicionar (*File->Add images from directory*) y el fichero *.gpx* con las coordenas recogidas por el GPS (*File->Load tracks from file*)
7. Ahora desde la lista de fotos *botón derecho->Find locations->for all images*. Esto lanzará el matching entre el timestamp (hora:minuto) de la informacion guardada en la foto (exif) con la posición GPS registrada en es momento.
8. Una vez todas las fotos han sido correlacionadas (si no, podemos hacerlo manualmente ya que nos posiciona la foto en un mapa).
9. Ahora solo falta fijar estas coordenadas a la información del foto (exif). Para ello desde el menú *File->Save new locations->All images* (este proceso no modifica la foto original, crea una copia con el mismo nombre y los nuevos metadatos de GPS y la antigua la renombra con el sufijo *\_original*)
10. Una vez acaba este proceso cerramos el Geotag.
11. Desde el terminal que teníamos abierto, borramos los ficheros originales:


    ```auto
    rm *_original
    ```
12. Ejecutamos ***jhead*** para restaurar la fecha y hora original de la foto (puedes ver que el nuevo fichero tiene la fecha y hora de hoy y no de cuando fué tomada). Básicamente *jhead* lee la información exif de cuando se tomó la foto y la pone como fecha de creación del fichero:


    ```auto
    jhead -ft *.JPG
    ```
13. Ahora ya toca clasificar las fotos.
