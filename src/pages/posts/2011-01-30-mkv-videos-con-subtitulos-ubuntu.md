---
layout: ../../layouts/post.astro
title: "MKV: videos con subtítulos (Ubuntu)"
pubDate: 2011-01-30
description: "Una solución a un problema que me he encontrado: reproducir por  (con  como servidor) ficheros MKV con subtitulos en la plataforma VodafoneT"
author: "akirasan"
isPinned: false
excerpt: "Una solución a un problema que me he encontrado: reproducir por  (con  como servidor) ficheros MKV con subtitulos en la plataforma VodafoneT"
image:
  src: ""
  alt: "MKV: videos con subtítulos (Ubuntu)"
tags: []
---

Una solución a un problema que me he encontrado: reproducir por [DLNA](http://es.wikipedia.org/wiki/Digital_Living_Network_Alliance) (con [minidlna](http://minidlna.sourceforge.net/) como servidor) ficheros MKV con subtitulos en la plataforma VodafoneTV, ha sido la de incrustar los subtítulos mediante las herramientas de ***mkvtoolnix***, mas concretamente la *mkvmerge*. Instalación del paquete:

> ```auto
> apt-get install mkvtoolnix
> ```

Se puede instalar también la interface gráfica ***mkvtoolnix-gui***, pero como el comando es muy sencillo se puede hacer directamente en la consola, como a mi me gusta:
> ```auto
> mkvmerge <fichero.mkv> <fichero.srt> -o <fichero_salida.mkv> --output-charset UTF-8
> ```

En un par de minutos (en un Atom) ya tienes integrados los subtítulos en el contenedor del [Matroska](http://www.matroska.org/). Por cierto, si no sale por defecto los subtítulos cuando se reproduce por DLNA, hay que seleccionarlo (según reproductor).
