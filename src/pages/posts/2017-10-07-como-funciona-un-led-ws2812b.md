---
layout: ../../layouts/post.astro
title: "¿Cómo funcionan los LED WS2812B?"
pubDate: 2017-10-07
description: "La pregunta/duda salió en una sesión de cacharreo de , así que me puse a buscar información y la verdad es que cuando lees información sobre"
author: "akirasan"
isPinned: false
excerpt: "La pregunta/duda salió en una sesión de cacharreo de , así que me puse a buscar información y la verdad es que cuando lees información sobre"
image:
  src: "/content/images/2017/10/upload-1507330537.jpg"
  alt: "¿Cómo funcionan los LED WS2812B?"
tags: []
---

La pregunta/duda salió en una sesión de cacharreo de [Ripolab Hacklab](http://ripolab.org), así que me puse a buscar información y la verdad es que cuando lees información sobre el funcionamiento de un **WS2812b** te das cuenta de que es realmente ingenioso. Para empezar vamos a ver de que están compuestos:

* Cada "LED" dispone de un integrado que almacena 3 bytes (el WS2812b)
* Tiene en su interior 3 LEDs tipo 5050 (5.0 x 5.0 mm) con los colores básicos RGB (Red-Green-Blue). Combinando estos colores podemos representarlos todos.
* Entrada de alimentación Vcc/GND y Datos (DataIn). Estas entradas se replican como salida Vcc, GND y DataOut para el siguiente LED, si lo hay.

En esta imagen aumentada se puede ver el integrado WS2812b y los tres *microleds* tipo 5050.

![ws2812b_led](/content/images/2017/09/ws2812b_led.jpg)

Hemos dicho que el integrado WS2812b almacena 3 bytes (24 bits), la razón es porque utiliza 1 byte por cada led/pixel de color RGB. Con un byte (o 8 bits) se puede almacenar valores de 0 a 255, eso significa que cada *microled/pixel* RGB puede tener hasta 256 niveles. Por lo tanto si combinamos los tres colores RGB, podemos representar mas de 16 millones de colores posibles. No está mal!!! eh??

Bien, pero ¿cómo podemos decirle a cada LED que tenga un color determinado y todos los datos pasan por solo cable?. La solución es super sencilla: cuando un LED recibe un flujo de bytes, almacena los últimos bytes recibidos y trasmite los que contenía al siguiente LED. Finalmente, con una señal de que se llama “resetcode” cada LED muestra el último valor que tiene almacenado. ¿Se entiende?, no, a ver...con un ejemplo tal vez sea mas sencillo:

Imaginamos que tenemos una tira de 5 LED's y queremos encender el LED1 de rojo, el LED3 de color verde y el LED5 de color azul, dejando el resto apagados. Pues bien, la idea es lanzar una ristra de bits de esta forma:  
![ws2812b_led_ejemplo1](/content/images/2017/10/ws2812b_led_ejemplo1.JPG)

Esto quiere decir que siempre vamos a enviar toda la información para cada uno de los LEDs, aunque estén apagados.

![ws2812b_led_traspaso_bits](/content/images/2017/10/ws2812b_led_traspaso_bits.JPG)

Luego una vez todos tienen la información, se lanza la señal *resetcode* y todos muestran la información que tienen almacenada:  
![ws2812b_led_traspaso_bits_resetcode](/content/images/2017/10/ws2812b_led_traspaso_bits_resetcode.JPG)

Esta genial idea permite hacer configuraciones de múltiples LED, en los que únicamente tenemos que comunicarnos con el primero de ellos y cada LED se actúa de transmisor de la secuencia a los LED posteriores. Además permite que podamos encadenar o dividir tiras de LED y cualquier fragmento seguirá funcionando porque todos los LED tienen exactamente el mismo comportamiento. Además cada LED cuando transmite al siguiente la señal, realiza una reconstrucción, es decir que de esta forma no se distorsiona o se acumula ruido para los siguientes LEDs. Esto permite también poder alimentar tiras de más de 5m sin necesidad de dispositivos que amplifiquen la señal. Unas pequeñas maravillas!!!

![ws2812b_esquema](/content/images/2017/09/ws2812b_esquema.jpg)

La documentación técnica la podéis encontrar aquí en el [Datasheet](http://www.seeedstudio.com/document/pdf/WS2812B%20Datasheet.pdf)
