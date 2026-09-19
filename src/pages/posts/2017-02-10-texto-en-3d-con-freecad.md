---
layout: ../../layouts/post.astro
title: "Texto en 3D con FreeCAD"
pubDate: 2017-02-10
description: "Desde que tengo una impresora 3D (bueno dos, ya explicaré en otro post ;) ), el diseño en 3D con una herramienta es prácticamente inevitable"
author: "akirasan"
isPinned: false
excerpt: "Desde que tengo una impresora 3D (bueno dos, ya explicaré en otro post ;) ), el diseño en 3D con una herramienta es prácticamente inevitable"
image:
  src: "/content/images/2017/02/akirasan3D_freecad_text.jpg"
  alt: "Texto en 3D con FreeCAD"
tags: ["freecad", "3dprinting"]
---

Desde que tengo una impresora 3D (bueno dos, ya explicaré en otro post ;) ), el diseño en 3D con una herramienta es prácticamente inevitable. Yo utilizo **[FreeCAD](https://www.freecadweb.org/?lang=es_ES)**, porque aprendí con [los videotutoriales en Youtube](https://www.youtube.com/watch?v=2_DbFzFV9D4&list=PLmnz0JqIMEzWQV-3ce9tVB_LFH9a91YHf) de [@Obijuan\_cube](https://twitter.com/Obijuan_cube) que **son geniales!!!**, están muy bien explicados y cualquiera puede ser capacitado para realizar sus diseños en 3D.

Os voy a explicar cómo genero yo texto en 3D mediante una opción que tiene FreeCAD. Son pasos muy sencillos.

#### 1. Seleccionar el tipo de letra

Antes de comenzar tenemos que tener el fichero .ttf (TrueType Font) del tipo de letra que vamos a utilizar en nuestro diseño. Yo utilizo las fuentes proporciona el repositorio de [Google Fonts](https://fonts.google.com/).

![](/content/images/2017/02/freecad_3dletras_001.png)

![](/content/images/2017/02/freecad_3dletras_002.png)

Una vez descargadas, las vamos a descomprimir para disponer el fichero .ttf a tiro para poder utilizar en FreeCAD:

![](/content/images/2017/02/freecad_3dletras_003.png)

#### 2. Generar objeto/dibujo plano en 2D

Abrimos FreeCAD y seleccionaremos el entorno **Draft**

![](/content/images/2017/02/freecad_3dletras_005.png)

Una vez ahí, tenemos un icono en forma de ***S*** (Crear una cadena de texto en formas).

![](/content/images/2017/02/freecad_3dletras_006.png)

En ese momento vamos a seguir los diferentes pasos que nos indica el *asistente* para generar el texto. Primero vamos a seleccionar el punto donde queremos fijar nuestro texto:

![](/content/images/2017/02/freecad_3dletras_007.png)

Los siguientes tres pasos los he simplificado un poco en una sola imagen:

* **1** El texto que queremos poner
* **2** La altura, por defecto sale 100mm. Se puede cambiar ahora o mas adelante.
* **3** *Seguimiento* es la separación entre las letras, yo por defecto lo dejo a 0mm.

![](/content/images/2017/02/Selecci-n_006.png)

Aquí llega el momento de buscar el fichero .ttf que definirá el tipo y estilo de nuestro texto.

![](/content/images/2017/02/freecad_3dletras_011.png)

Una vez seleccionado de la ruta de acceso, darle al *Enter* para no perderlo ;)

![](/content/images/2017/02/freecad_3dletras_012.png)

Con ésto, hemos generado un texto plano en 2D. Ahora nos faltará poder darle el volumen esperado:

![](/content/images/2017/02/freecad_3dletras_013.png)

#### 3. Extrusar la forma 2D

Ahora ya podemos extrusar y generar un objeto 3D sólido. Para ello cambiaremos de entorno de trabajo y nos iremos a **Part**. Seleccionamos el objeto de nuestro texto y utilizamos el icono de Extrusar:

![](/content/images/2017/02/freecad_3dletras_014.png)

Le damos mediante el parámetro *Z* la altura y marcamos la creación del un objeto sólido.

![](/content/images/2017/02/freecad_3dletras_015.png)

**POP!!!** ya tenemos nuestro texto en 3D y listo para imprimir (por ejemplo ;) )

![](/content/images/2017/02/freecad_3dletras_016.png)

Espero que os sirva de ayuda!!!
