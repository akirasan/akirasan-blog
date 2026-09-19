---
layout: ../../layouts/post.astro
title: "Update a Wordpress 2.1.3"
pubDate: 2007-05-06
description: "A parte del cambio visual del blog, una de las cosas que quería hacer en el servidor es limpieza y sobretodo actualizar el Wordpress 2.0 a l"
author: "akirasan"
isPinned: false
excerpt: "A parte del cambio visual del blog, una de las cosas que quería hacer en el servidor es limpieza y sobretodo actualizar el Wordpress 2.0 a l"
image:
  src: ""
  alt: "Update a Wordpress 2.1.3"
tags: []
---

A parte del cambio visual del blog, una de las cosas que quería hacer en el servidor es limpieza y sobretodo actualizar el Wordpress 2.0 a la 2.1.3. Mi idea ha sido tener una 2.1.3 lo mas limpia posible (para la futura 2.2). Para ello he seguido estos pasos (previamente lo he tenido que hacer en una instalación local, no me la iba a jugar,...):

1. Crear una base de datos y un directorio nuevo para la WP 2.1.3.
2. Instalar el WP 2.1.3 en el directorio y base de datos de forma standard (de esta forma no interfiero en la instalación actual).
3. Ahora ya tengo en el mismo servidor una WP 2.0 y una 2.1.3.
4. Exporto las tablas del WP 2.0 (son 10 tablas que en mi caso tenían el prefijo "wp2\_", fácilmente reconocibles).
5. Borro las tablas "*vacias*" que me ha realizado la instalación WP 2.1.3, de su base de datos.
6. Importo las tablas de la versión 2.0 en la base de datos de la versión 2.1.3.
7. Ahora realizo el proceso de upgrade de la 2.1.3, y listo!!!,...ya tengo una 2.1.3 limpia, sin plugin's, modificaciones raras de prueba, etc,...
8. Una vez tengo el contenido migrado, falta instalar el tema y los plugins que realmente utilizo, pero ahora para la 2.1.3. Como utilizo el [ImageManager](http://www.soderlind.no/archives/2006/01/03/imagemanager-20/ "http://www.soderlind.no/archives/2006/01/03/imagemanager-20/") para incluir las imágenes en los post's, lo instalo de cero y decido cambiar el directorio de las imágenes por otro. Este cambio me requiere modificar en todos los post's que hay en la BD la URL de acceso, para ello utilizo la siguiente sentencia SQL con el phpMyAdmin:
    `update wp_posts set post_content = replace(post_content,'<link viejo>','<link nuevo>');`
   Esto me permite cambiar en todas las entradas el literal de un texto por otro.

Ahora que ya tengo la infraestructura del blog lista, me dedicaré a migrar las galerías de fotos a algún servidor, que aún no tengo claro, y borraré todo lo que realmente ya no utilizo (bueno haré una copia y me la guardo de recuerdo,...jejeje).Solo tengo que esperar a que la [versión 2.2 de Wordpress aparezca en breve](http://www.blogherald.com/2007/04/25/wordpress-wednesday-news-4-million-themes-downloaded-wordpress-22-delayed-and-tons-of-new-fun-on-wordpresscom/ "http://www.blogherald.com/2007/04/25/wordpress-wednesday-news-4-million-themes-downloaded-wordpress-22-delayed-and-tons-of-new-fun-on-wordpresscom/"),...
