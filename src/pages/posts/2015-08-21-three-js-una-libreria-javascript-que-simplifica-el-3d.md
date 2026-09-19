---
layout: ../../layouts/post.astro
title: "Three.JS una librería JavaScript que simplifica el 3D"
pubDate: 2015-08-21
description: "es una de esas librería que en cuento la ves por primera vez dices: *\"Esto lo tengo que poner en algún sitio\"*, porque la verdad es que sorp"
author: "akirasan"
isPinned: false
excerpt: "es una de esas librería que en cuento la ves por primera vez dices: *\"Esto lo tengo que poner en algún sitio\"*, porque la verdad es que sorp"
image:
  src: "/content/images/2015/08/threejs_ejemplo1-1.JPG"
  alt: "Three.JS una librería JavaScript que simplifica el 3D"
tags: ["tools", "javascript"]
---

[Three.JS](http://threejs.org/) es una de esas librería que en cuento la ves por primera vez dices: *"Esto lo tengo que poner en algún sitio"*, porque la verdad es que sorprende lo sencillo de su uso e implementación y sorprende también los resultados.

##### ¿Pero que es Three.JS?#####

[Three.JS](http://threejs.org/) como ya hemos comentado es una librería JavaScript de fácil implementación y uso, ¿pero para qué?, pues nada mas y nada menos que el tratamiento de las 3D en el mundo web. Algo que siempre está lleno de complejos algoritmos matemáticos, modelos de renderización, software potentes que se comen la CPU y GPU,... pues Three.JS quiere hacerlo todo mas sencillo y ligero para que podamos incluirlo en nuestras creaciones web.

Las características que me han llamado la atención de esta librería son:

* Posibilidad de renderizar con: [WebGL](https://es.wikipedia.org/wiki/WebGL), [<canvas>](https://es.wikipedia.org/wiki/Canvas_(HTML)), [<svg>](https://es.wikipedia.org/wiki/Scalable_Vector_Graphics), CSS3D, DOM
* Poder crear escenas en las que incluir y eliminar objetos, efectos de luz (ambiente, spots, sombras...), cámaras, perspectiva, paths, etc en tiempo real.
* Generación de animación
* Utilización de materiales y texturas.
* Objetos (meshes, partículas, sprites, lineas,...) y figuras geométricas (planos, cubos, esferas, textos en 3D, etc)
* Permite cargar ficheros como imágenes, vídeos, JSON y escenas ya generadas
* Multitud de funciones matemáticas para el manejo de las 3D
* Export/Import de ficheros JSON compatibles desde herramientas como: Blender, CTM, FBX, 3D Max y OBJ

Pero una de las cosas que mas llama la atención y con la que vas a flipar un poco, es con su gran volumen de [DEMOS](http://threejs.org/examples/) y un [editor online (en fase beta)](http://threejs.org/editor/) que ayuda a la generación de tu escena.

[![demo1\_threejs](ht![demo1_threejs](/content/images/2015/08/threejs_ejemplo1-1.JPG)](/content/images/2015/08/threejs_ejemplo1-1.JPG)  
[![demo2\_threejs](ht![demo2_threejs](/content/images/2015/08/threejs_ejemplo2.JPG)](/content/images/2015/08/threejs_ejemplo2.JPG)

No he podido resistirme a hacer una prueba muy sencillita: [un cubo 3D](http://www.akirasan.net/content/images/2015/08/demo_threejs.html)

Puedes encontrar mucha mas información en su [wiki](https://github.com/mrdoob/three.js/wiki)
