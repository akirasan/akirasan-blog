---
layout: ../../layouts/post.astro
title: "Detectar el movimiento con OpenCV"
pubDate: 2016-06-30
description: "Una webcam, OpenCV instalado y un poco de Pyhton, es todo lo que te hace falta para generar un script capaz de analizar las imágenes de víde"
author: "akirasan"
isPinned: false
excerpt: "Una webcam, OpenCV instalado y un poco de Pyhton, es todo lo que te hace falta para generar un script capaz de analizar las imágenes de víde"
image:
  src: "/content/images/2016/06/opencv-python.png"
  alt: "Detectar el movimiento con OpenCV"
tags: ["python", "opencv"]
---

Una webcam, OpenCV instalado y un poco de Pyhton, es todo lo que te hace falta para generar un script capaz de analizar las imágenes de vídeo y detectar objetos en movimiento.

###### ¿Cómo es el algoritmo básico?

No voy a entrar en muchos detalles técnicos (básicamente porque los desconozco ;) ), pero el proceso de detectar zonas en movimiento pasa simplemente por comparar dos imágenes, y encontrar las diferencias entre ambas. Con este principio básico y lógico este algoritmo hace algo similar utilizando una serie de métodos y técnicas que lo optimizan y simplifica, ya que comparar *pixel a pixel* no sería viable ni óptimo. Por lo tanto los pasos que seguiremos serán:

1. Capturar una secuencia de tres imágenes.
2. Transformamos las imágenes a una escala de grises (esto mejora el rendimiento del proceso).
3. Reducimos la resolución de las imágenes previas para mejorar el rendimiento a la hora de *"comparar imágenes"* (imágenes con resoluciones altas hace que el proceso sea mas lento)
4. Comparamos las tres imágenes mediante diferencia de pixels.
5. Buscamos las zonas diferentes y las remarcamos o tenemos en cuenta aquellas que tienen una cierta relevancia, por ejemplo, si cambian 5 pixels, pues igual no sea relevante.

Cinco sencillos pasos,... ;). Bueno voy a tratar de explicar los diferentes puntos con trozos de código del script:

###### Paso 1 + Paso 2: Capturar una secuencia de tres imágenes en escala de grisis

Creo que éste es el paso mas sencillo que vamos a ver. Gracias a la función de OpenCV la captura de una imagen desde la cámara es sencillo, además podemos indicar ya el filtro que queremos utilizar, en este caso el `COLOR_RGB2GRAY` que nos pasa a escala de grises.

```auto
t_minus = cv2.cvtColor(cam.read()[1], cv2.COLOR_RGB2GRAY)
t = cv2.cvtColor(cam.read()[1], cv2.COLOR_RGB2GRAY)
t_plus = cv2.cvtColor(cam.read()[1], cv2.COLOR_RGB2GRAY)
```

Pero cómo luego vamos a querer mostrar el resultado en color una ventana adicional, vamos a guardarnos el siguiente frame **en color**, pero ya redimensionado a 320x240. Utilizamos la función `cv2.resize` y le pasamos la imagen y un tipo de algoritmo de reescalado, en este caso un `INTER_CUBIC`: `interpolation = cv2.INTER_CUBIC` (uff, que lío!!!):

```auto
original = cv2.resize(cam.read()[1],(320,240), interpolation = cv2.INTER_CUBIC)
```

El porqué estamos capturando tres frames (imágenes) en lugar de dos, que sería lo mas habitual, es porque buscando las diferencias entre tres imágenes el resultado es mas óptimo.

###### Paso 3 + Paso 4: Reducimos la resolución de las imágenes y generamos la diferencias

Vamos a generar una imagen que se llamará `imagen_delta`, esta imagen va a contener la diferencia en pixels de las tres imágenes que hemos capturado. Para ello enviaremos a la función `diffImg` las tres imágenes en escala de grises, pero escaladas a una resolución mas baja de 320x240 para simplificar el proceso, así que volvemos a utilizar la función `cv2.resize` y el tipo de algoritmo: `interpolation = cv2.INTER_CUBIC`, para cada una de las imágenes `t_minus`, `t` y `t_plus`:

```auto
imagen_delta = diffImg(cv2.resize(t_minus,(320,240), interpolation = cv2.INTER_CUBIC), cv2.resize(t,(320,240), interpolation = cv2.INTER_CUBIC), cv2.resize(t_plus,(320,240), interpolation = cv2.INTER_CUBIC))
```

La función `diffImg` que hemos preparado recoge tres imágenes, las cuales trata cómo arrays y mediante le método de OpenCV `absdiff(p1, p2)` calcula la diferencia entre matrices: primero calculamos la diferencia entre la imagen `t_minus` y `t`, y luego entre `t` y `t_plus`, es decir, la idea es comparar imágenes: *el frame1 con el frame2 y luego el frame2 con el frame3*:

```auto
def diffImg(t0, t1, t2):
  d1 = cv2.absdiff(t2, t1)
  d2 = cv2.absdiff(t1, t0)
  return cv2.bitwise_and(d1, d2)
```

A partir de aquí tenemos dos matrices `d1` y `d2` con las diferencias, las cuales vamos a fusionar con un AND a nivel de bit, mediante la función `cv2.bitwise_and()`. Resultado final: una imagen (que llamamos `imagen_delta`) con los pixels que son diferentes.

###### Buscamos las zonas diferentes y las remarcamos

Una vez tenemos la imagen con los pixels que marcan las diferencias (`imagen_delta`), llega el momento localizarlas para poder marcarlas con un recuadro. Para antes de poder utilizar la función que nos permite realizar esto, vamos a tener que buscar los contornos de *las zonas* que nos han salido en la `imagen_delta`. Hemos creado una función llamada `marcar_zonas()` en la cual le pasamos la imagen con las diferencias y la imagen original (donde dibujaremos las zonas con movimiento):

```auto
imagen_zonas_marcadas = marcar_zonas(imagen_delta, original.copy())
```

...Y éste es el código de la función que nos va a marcar y preparar la imagen final:

Primero entender que `frame_mov` va a ser nuestro "delta" y `frame_original` la imagen *en color* donde vamos a marcar el resultado:

```auto
def marcar_zonas(frame_mov, frame_original):
  frame_mov = cv2.GaussianBlur(frame_mov, (21, 21), 0)
  limites = cv2.threshold(frame_mov, 5, 255, cv2.THRESH_BINARY)[1]
  limites = cv2.dilate(limites, None, iterations=2)
  contours, hierarchy = cv2.findContours(limites.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
  movimiento_detectado = False
  for c in contours:
   if cv2.contourArea(c) < 800:
     continue
   (x, y, w, h) = cv2.boundingRect(c)
   print x,y,w,h
   cv2.rectangle(frame_original, (x, y), (x + w, y + h), (0, 0, 255), 1)
   movimiento_detectado = True
```

Por partes: **Primero** vamos a coger la imagen `delta` y le vamos a pasar un filtro *GaussianBlur*, esto lo hacemos para tener mejores *formas* y poder eliminar pixels *sueltos*. Aquí podéis jugar con los valores para poder adaptar a la situación:

```auto
  frame_mov = cv2.GaussianBlur(frame_mov, (21, 21), 0)
```

Una vez tenemos la imagen lista vamos a detectar los contornos con estos métodos de OpenCV:

```auto
  limites = cv2.threshold(frame_mov, 5, 255, cv2.THRESH_BINARY)[1]
  limites = cv2.dilate(limites, None, iterations=2)
  contours, hierarchy = cv2.findContours(limites.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
```

Vamos a tener finalmente un Array llamado `contours` que cómo se puede entender, tendremos los contornos de las *formas* que el algoritmo de OpenCV ha detectado. Recorriendo esa Array vamos a calcular el área de ese contorno y determinar si es válido o no para nosotros. En este caso yo he establecido que el valor del área de las formas tiene que ser de 800, pero aquí vuelve a ser un parámetro a ajustar en función de la situación:

```auto
  for c in contours:
   if cv2.contourArea(c) < 800:
     continue
   (x, y, w, h) = cv2.boundingRect(c)
   print x,y,w,h
   cv2.rectangle(frame_original, (x, y), (x + w, y + h), (0, 0, 255), 1)
   movimiento_detectado = True
```

Si el área de la zona que hemos detectado cumple con el valor de 800, vamos a utilizar `cv2.boundingRect(c)` para determinar las coordenadas de inicio `x` y `y`, y el ancho `w` y el alto `h`, por lo que podríamos dibujar un rectángulo mediante las coordenadas `(x, y)` , `(x + w, y + h)`. Y eso lo hacemos mediante el método `cv2.rectangle(imagen, (x1, y1), (x2, y2), color, ancho_linea)`

Bueno, a parte de remarcar la zona detectada, también he añadido en la imagen la fecha y hora en la que se detecta el movimiento (*nota mental: desarrollar un sistema de vigilancia*). Y para poder tener constancia grabamos la imagen (*nota mental: posible sistema de notificaciones de seguridad*).

Espero que os pueda ayudar todo este tocho post que me ha quedado. Si lo queréis ver en acción, aquí os dejo un vídeo demostración:

Y para tener todo el código Python, en mi [canal GitHub](https://github.com/akirasan/move_detector) que he abierto recientemente.
