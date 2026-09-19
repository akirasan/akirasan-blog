---
layout: ../../layouts/post.astro
title: "Intel® Open Image Denoise en Blender 2.81"
pubDate: 2019-08-30
description: "Llevo días viendo ejemplos de lo que está consiguiendo una nueva funcionalidad para reducir el ruido generado en una imagen renderizada con"
author: "akirasan"
isPinned: false
excerpt: "Llevo días viendo ejemplos de lo que está consiguiendo una nueva funcionalidad para reducir el ruido generado en una imagen renderizada con"
image:
  src: "/content/images/2019/08/portada-denoise2.png"
  alt: "Intel® Open Image Denoise en Blender 2.81"
tags: ["blender", "3D"]
---

Llevo días viendo ejemplos de lo que está consiguiendo una nueva funcionalidad para reducir el ruido generado en una imagen renderizada con pocos samples (muestreos) en Blender 2.81

He querido hacer mis propias pruebas, ver cómo funciona y analizar los resultados, y la verdad es que he flipado bastante con el resultado final. Pero antes de ver los resultados, vamos a conocer un poco mas que es esto del *Open Image Denoise*

## Open Image Denoise

**Intel® Open Image Denoise** es una **librería de código abierto** de filtros avanzados de **eliminación de ruido para imágenes generadas** (renders, ray tracing).

La idea de *Open Image Denoise* es que **podamos reducir los tiempos de renderizado** reduciendo el número de samples (muestras) utilizadas y limpiar la imagen generada del ruido producido por ese **render** ***low-cost***.

Es **una librería entrenada mediante Deep Learning con redes neuronales** para ser mas eficiente y conseguir los mejores resultados. Esto es una auténtica flipada!!!

Podéis encontrar mucha mas información y descargar el código fuente es su página de Github: <https://openimagedenoise.github.io/>

## Descargar la versión Blender 2.81

La versión 2.81 (actualmente en desarrollo) se puede descargar de la página oficial, en el apartado de *Download* pero "*Experimental*". Aquí os dejo el link: <https://builder.blender.org/download/>

![](/content/images/2019/08/render_iodenoise00.png)

## Denoising vs Open Image Denoise

El objetivo de las pruebas es comparar los resultados utilizando la opción normal de *Denoising* que lleva Blender, contra la futura característica de *Open Image Denoise* que se aplica desde el *Composer* al finalizar el renderizado de la escena. **Utilizando siempre el mismo motor de renderizado *Cycles***.

**Veremos tanto el tiempo de renderizado cómo el resultado final**. Normalmente **cuantos mas samples utilizamos, mas tiempo de proceso es necesario** y mejor resultado final obtendremos, pero **esto puede cambiar** con *Open Image Denoise*.

En el siguiente cuadro resumen vamos a poder ver una comparativa del tiempo en ***minutos:segundos*** obtenido en cada prueba:

| Número samples | Sin Denoise | Denoising | Open Image Denoise |
| --- | --- | --- | --- |
| 128 | 09:33 | 10:00 | 09:47 |
| 64 | 04:39 | 06:19 | 04:50 |
| **16** | **01:02** | **02:12** | **01:33** |
| 8 | 00:48 | 01:49 | 01:03 |
| 4 | 00:43 | 01:32 | 00:50 |
| 1 | 00:30 | 01:18 | 00:36 |
|  |  |  |  |

Cómo veréis más adelante, el objetivo de *Open Image Denoise* es obtener una imagen clara con el menor número de samples posibles, reduciendo el tiempo de renderizado, por lo que no tiene sentido comparar tiempos con el mismo número de samples.

## Activación de Denoising y de Open Image Denoise

Para cada prueba he activado una de las dos opciones para comparar cómo se comportan.

La opción clásica de *Denoising* se encuentra en este punto de menú. La he activado y dejado con los valores por defecto.

![](/content/images/2019/08/render_iodenoise02.png)![](/content/images/2019/08/render_iodenoise03.png)

Para configurar y usar el *Open Image Denoise*, tenemos que acceder la parte de *Compositing*, activar el uso de *Nodes* (nodos).

![](/content/images/2019/08/render_iodenoise08.png)![](/content/images/2019/08/render_iodenoise09-1.png)

Cómo podréis ver nos salen por defecto dos nodos por defecto: *Render Layer* y *Composite*. El primero hace referencia la resultado final tras realizar el renderizado, y el segundo (que recibe la imagen del primero) es el resultado final de imagen.

Para poder nutrir de mas información al nodo de *Open Image Denoise,* que añadiremos después, primero vamos a activar la opción *Denoising Data*, la cual nos mostrará mas conectores en el nodo de *Render Layer*.

![](/content/images/2019/08/render_iodenoise10.png)

Ahora añadimos el nodo *Denoise*, que se encuentra en el apartado de *Filter*.

![](/content/images/2019/08/render_iodenoise11.png)

El nodo *Denoise* será el que conectaremos entre medio de *Render Layer* y *Composite* para que de esta forma se aplique el filtro una vez realizado el render:

![](/content/images/2019/08/render_iodenoise12.png)

## Resultado de las imágenes

[Las imágenes generadas las he dejado disponibles en Github](https://github.com/akirasan/Blender/tree/master/Intel%20OpenImage%20Denoise%20Test%20Blender%202.81), sin comprimir en PNG para que podáis ampliar y comparar por vosotros mismos el resultado de la imagen final. Aquí vamos a hacer zoom en una zona concreta de la imagen para ver las diferencias.

A 128 samples ya podemos ver las diferencias: en el blanco de la pared y en el lateral que vemos del cubo. Con *Denoising* claramente se ve que hace lo que puede y ya comienzan a verse manchas, mientras que con *Open Image Denoise* el resultado es aceptable:

![](/content/images/2019/08/128.jpg)

Bajamos a 64 samples y aquí el tema se pone interesante. *Denoising* se va degradando, pero en cambio *Open Image Denoise* parece que ni se inmuta del incremento del ruido. Además bajamos a la mitad en tiempo de renderizado:

![](/content/images/2019/08/64.jpg)

Vamos a darle mas caña y bajar a 16 samples. El ruido ya comienza a ser muy visible y *Denoising* sufre las consecuencias. Por otro lado *Open Image Denoise* parece que sigue dando un buen resultado y el tiempo de renderizado es brutal, respecto al de 128 samples:

![](/content/images/2019/08/16.jpg)

A por mas!!! 8 samples:

![](/content/images/2019/08/8.jpg)

A lo loco!!! a 4 samples

![](/content/images/2019/08/4.jpg)

Lo impensable, render con un solo sample!!!. Aunque Open Image Denoise consigue hacer lo imposible!!!

![](/content/images/2019/08/1.jpg)

Está claro que *Open Image Denoise* hace un trabajo brutal e increíblemente bueno. Aquí podéis ver el resumen y poder comparar. Creo que entre 16 y 8 samples se puden conseguir resultados aceptables, aunque perdemos algo de definición de las formas, pero ganamos en tiempo de renderización:

![](/content/images/2019/08/openimagedenoise_compare.jpg)

En fin, sacad vuestras propias conclusiones, pero yo creo que esta nueva funcionalidad va a ayudar a obtener buenas escenas con menor tiempo de cálculo en el render.
