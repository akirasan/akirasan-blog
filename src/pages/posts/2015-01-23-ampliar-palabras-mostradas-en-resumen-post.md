---
layout: ../../layouts/post.astro
title: "Ampliar palabras mostradas en resumen post"
pubDate: 2015-01-23
description: "El tema por defecto que trae Ghost está muy bien (), pero para mi gusto el resumen de los post que aparece en la página principal es un poco"
author: "akirasan"
isPinned: false
excerpt: "El tema por defecto que trae Ghost está muy bien (), pero para mi gusto el resumen de los post que aparece en la página principal es un poco"
image:
  src: ""
  alt: "Ampliar palabras mostradas en resumen post"
tags: ["Linux", "Ghost"]
---

El tema por defecto que trae Ghost está muy bien ([Casper](http://allghostthemes.com/casper/)), pero para mi gusto el resumen de los post que aparece en la página principal es un poco corto. Es por ello que si queremos que muestre algo mas de información tenemos que modificar el comportamiento de este tema y por lo tanto su codificación.

La configuración la encontraremos en un fichero llamado `loop.hbs`que en el tema de Casper se encuentra en el path: `/ghost/content/themes/casper/partials/`

Modificamos la linea que contiene el valor por defecto, que **son 26 palabras**:

```auto
<section class="post-excerpt">
    <p>{{excerpt words="26"}} <a class="read-more" href="{{url}}">&raquo;</a></p>
</section>
```

Por ejemplo, lo podemos dejar en unas 46 (es como lo tengo yo):

```auto
<section class="post-excerpt">
    <p>{{excerpt words="46"}} <a class="read-more" href="{{url}}">&raquo;</a></p>
</section>
```

Una vez modificado el fichero, **tenemos que reiniciar el servicio Ghost** (aquí cada uno como lo tenga configurado, yo lo tengo como servicio).
