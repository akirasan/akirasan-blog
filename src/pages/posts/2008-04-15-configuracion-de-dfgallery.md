---
layout: ../../layouts/post.astro
title: "Configuración de dfGallery"
pubDate: 2008-04-15
description: "Bueno, debido a que he encontrado un par de comentarios relativos a como configurar esta fotogalería en flash para que se pueden cargar foto"
author: "akirasan"
isPinned: false
excerpt: "Bueno, debido a que he encontrado un par de comentarios relativos a como configurar esta fotogalería en flash para que se pueden cargar foto"
image:
  src: ""
  alt: "Configuración de dfGallery"
tags: []
---

Bueno, debido a que he encontrado un par de comentarios relativos a como configurar esta fotogalería en flash para que se pueden cargar fotos que no estén alojadas en el propio servidor, paso a explicaros mas o menos como lo he solucionado yo.

Básicamente he creado una página en Wordpress basada en un template de página que contiene la llamada normal para invocar el dfGallery. Éste es [el template que utilizo](http://www.akirasan.net/t_fotogalteria.php.txt "t_fotogalteria.php.txt") para mi página de fotogalería.

El secreto está en la definición del fichero ***gallery.xml***, que se utiliza para configurar la fotogalería. Esta es la forma en la que tengo referenciadas cada uno de los albunes:

> ```auto
> <!-- this node contains all the albums -->
> ```
>
> ```auto
> <albums>
> ```
>
> ```auto
> <album title="B&W" description="Fotografias en blanco y negro" type="zooomr" url="bw.xml.php"></album>
> ```
>
> ```auto
> <album title="motoGP 2007" description="Gran Premio Motociclismo de Catalunya" type="zooomr" url="motogp2007.xml.php"></album>
> ```
>
> ```auto
> <album title="Naturaleza I. Macro" description="Fotografia tomadas con macro" type="zooomr" url="naturaleza.xml.php"></album>
> ```
>
> ```auto
> <album title="Laberint dHorta" description="El Laberint dHorta" type="zooomr" url="laberinto.xml.php"></album>
> ```
>
> ```auto
> </albums>
> ```

Como veis la URL hace referencia a un fichero PHP, por ejemplo ***bw.xml.php***, este fichero lo que hace es cargar cada una de las fotografías del servidor externo. Os dejo [aquí un copia de este fichero](http://www.akirasan.net/bw.xml.php.txt "bw.xml.php.txt") para que veáis el código PHP que generé.

Ya se que el método es un poco *chungo* y no muy dinámico (ya que hay que indicar cada una de las fotografía a cargar), pero vamos, funciona. De esta forma podéis cargar fotos externas de otros servidores que no sean los soportados por dfGallery.
