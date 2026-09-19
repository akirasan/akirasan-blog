---
layout: ../../layouts/post.astro
title: "IR + Arduino = control del aire acondicionado"
pubDate: 2020-07-31
description: "Jugando con infrarrojos y Arduino. Si tu idea es clonar los comandos que salen de un mando a distancia, puedes empezar con esta entrada de m"
author: "akirasan"
isPinned: false
excerpt: "Jugando con infrarrojos y Arduino. Si tu idea es clonar los comandos que salen de un mando a distancia, puedes empezar con esta entrada de m"
image:
  src: "/content/images/2020/07/imagen_portada.jpg"
  alt: "IR + Arduino = control del aire acondicionado"
tags: ["Arduino", "ESP8266", "DIY"]
---

Jugando con infrarrojos y Arduino. Si tu idea es clonar los comandos que salen de un mando a distancia, puedes empezar con esta entrada de mi blog.

La idea de mi proyecto es domotizar y controlar un split de aire acondicionado. Tiene un mando de control remoto por infrarrojos (IR), por lo que tiene que ser sencillo poder clonar los comandos de cada botón y reproducirlos. Todo mediante Arduino, para empezar,...y luego con un ESP2866 o ESP32 para domitizarlo por completo, conectándolo a la WiFi.

Para leer y clonar comandos voy a utilizar la librería [IRremote](https://github.com/z3t0/Arduino-IRremote), que fácilmente nos ayuda con IRsend y IRrecv. Además soporta diversos protocolos: Aiwa, BoseWave, Denon, Dish, JVC, Lego, LG, MagiQuest, Mitsubishi, Panasonic, RC5, RC6, Samsung, Sanyo, Sharp, Sony, Whynter.

Si desconocemos el protocolo que necesitamos, no hay problema podemos leer y enviar códigos en formato raw, de tal forma que clonamos los códigos tal cual son recibidos.

## ¿Qué material necesitamos?

Prácticamente necesitamos un **LED emisor de infrarrojos**, y **LED receptor** tipoTL1838 y un Arduino (o clon).

**LED emisor de infrarrojos**, es muy similar a cualquier diodo led convencional, tiene sus dos patas y se conecta de la misma forma: ánodo y cátodo. Aquí la única diferencia, es que no vamos a poder ver la luz infrarroja que emite (ya sabéis que hay un truco para ver si funciona y emite, y es enfocarlo con la cámara del móvil, cómo si le fuéramos a hacer una foto, así a través de la pantalla y cámara del móvil si que podemos ver la luz que emite).

**LED receptor** tipo TL1838. Suele tener tres patas: Vcc, GND y salida. La salida será la que conectaremos a nuestro pin de entrada de Arduino. Y la alimentación deberá estar entre 3v y 5v, por lo que es ideal para conectarlo directamente sobre un Arduino.

![](/content/images/2020/07/image-10.png)

El esquema básico recomienda la utilización de un condensador y resistencia en la salida con la idea de tener un lectura mas estable. La verdad es que yo lo que conectado directamente y no he tenido problema en clonar los códigos. También las condiciones de temperatura e iluminación pueden afectar, pero vamos,...no soy muy purista con estas cosas a menos que no funcione cómo se espera.

![](/content/images/2020/07/image-11.png)

Otro característica de este receptor es que un 18**38**, trabaja a 38Khz, que suele ser un estándar y la gran mayoría de fabricantes utilizan esta frecuencia.

![](/content/images/2020/07/image-12.png)

## Programa básico de lectura

En los ejemplos de la librería podemos encontrar un ejemplo básico para lectura de un receptor IR. Se trata de leer los códigos que un mando remoto envía, para ello podemos utilizar **IRrecvDumpV2.ino** un ejemplo que nos da bastante información en RAW, es decir, sin codificar ni pasar por los protocolos de las diferentes marcas, información plana.

Por ejemplo, leer el mando de la tele, modelo SAMSUNG:

![](/content/images/2020/07/image-5.png)

Y cuando decodifico el mando del aire acondicionado HIYASU es reconocido cómo **UNKNOWN** (pero no es un problema):

![](/content/images/2020/07/image-6.png)

No es problema porque la librería *IRremote.h* nos permite capturar y enviar en modo RAW, y para el objetivo de mi proyecto es mas que suficiente.

![](/content/images/2020/07/image-13.png)

## Programa básico de envío

Me voy a centrar en el programa básico de envío en RAW, ya que mi propósito no va a ser decodificar el mensaje ni entenderlo, simplemente clonar lo que envía el mando original. Entonces podemos encontrar **IRsendRawDemo.ino**.

![](/content/images/2020/07/image-7.png)

Muy sencillo, vamos a definir una array tipo unsigned int y le vamos a asignar los códigos que hemos leído gracias a **IRrecvDumpV2.ino**. Por ejemplo: leemos un código, y ahí ya nos propone la estructura unsigned int rawData[67].

![](/content/images/2020/07/image-8.png)

Solamente tenemos que copiar eso y ponerlo en nuestro código, cambiando el ***rawData[67]*** por algo mas representativo, por ejemplo, ***subir\_volumen***:

![](/content/images/2020/07/image-9.png)

Una cosa a nivel de conexiones, a diferencia del programa que controla el receptor del IR, el programa que envía (o más bien la librería) utiliza el **PIN D3** en Arduino para enviar la info, así que ahí es donde deberemos conectar nuestro LED emisor.

![](/content/images/2020/07/image-14.png)

## Clonar códigos de un mando de aire acondicionado

El primer problema que me he encontré con la lectura del mando a distancia de mi aire acondicionado era que pulsando el mismo botón, recibía códigos distintos. Me dí cuenta que estos códigos cambiaban cuando el reloj del mando cambiaba de un minuto a otro. Por lo que entendía que podría enviar un *timestamp* en cada envío.

Hasta aquí no entendía que aunque le enviara un código clonado, aunque fuera con un *timestamp* igual no sería problema. Pero resulta que si, algo no funcionaba bien, el split de aire acondicionado no le gustaba lo que yo le enviaba. Raro, raro,...

Lancé la consulta por Twitter y alguien me dio una pista: resulta que normalmente los controles remotos del aire acondicionado envían toda la información que vemos en el mando, por lo tanto un botón, no solo envía su cambio, sino también el resto del estado, incluyendo la hora.

**Otra cosa que tenemos que tener en cuenta**, es que al enviar tanta información los códigos a clonar son mas largos. Un problema que tenía la librería *IRremote.h* (versión 2.4.0) es que la *array* que utiliza **está limitada a 100** y definida en RAWBUF:

![](/content/images/2020/07/image.png)

Cuando estaba escribiendo este post y probando esto, vi que la librería había cambiado y mejorado esta parte, en la **versión 2.5.0** de junio del 2020:

![](/content/images/2020/07/image-1.png)

Teniendo este pequeño problema para leer los códigos que enviaba el control remoto del aire acondicionado, estaba claro que tenía que modificar la librería o buscar algún otro programa que no estuviera limitado. Encontré éste <https://github.com/smokeyser/Remote2/blob/master/Contrib/rawirdecode.ino> que utilizaba parte de código de Adafruit. Podemos modificar tanto el pin donde conectamos el receptor IR cómo el tamaño del buffer:

![](/content/images/2020/07/image-2.png)

Así que podemos utilizar éste código o modificar la librería *IRremote.h*, para ello hay que modificar el ***#define RAWBUF*** en la 2.4.0 o el ***#define RAW\_BUFFER\_LENGHT*** para la versión 2.5.0, en el archivo *IRremoteInt.h*: (aquí modificado de 101 a 400)

![](/content/images/2020/07/image-3.png)

Ahora si que recibo toda la información, el buffer ya no está limitado a 100, ahora si que leo toda la información, el tamaño total que necesitaba era de 149 (si cuando hagas pruebas, ves que el tamaño del buffer es de 100,...sospecha y amplia el buffer por si acaso):

![](/content/images/2020/07/image-4.png)

## Solución rápida y práctica

Cómo he comentado y hemos visto, el código que emite mi mando de aire acondicionado no es muy estándar, así que no puedo decodificar y separar las diferentes funciones, a menos que invierta tiempo en analizar el patrón de la señal y la información que se envía, buscando diferencias,...en fin mucho lío, soy mas práctico.

Básicamente vamos a grabar condiciones o escenarios que nos parezcan adecuados y con eso es con lo que vamos a jugar. Por ejemplo, si normalmente pones el aire acondicionado **a 25 grados en modo automático, graba ese escenario**. Y luego un escenario de OFF. No hace falta nada mas, el split controlará la temperatura y se irá regulando:

![](/content/images/2020/07/image-18.png)

Yo he querido controlar algunos escenarios mas, por ejemplo, el modo ventilador y sus tres potencias posibles:

![](/content/images/2020/07/image-17.png)

Entonces estoy grabando esos códigos tal cual se muestran en el display del mando. Recordad que lo que envía el mando es todo lo que vemos en el display.

![](/content/images/2020/07/image-15.png)

Realmente el ON\_fan1 no es necesario, ya que cualquier comando que enviemos al split y que no sea un OFF será interpretado y ejecutado, pero bueno...

## Funciones básicas

Con este simple control que tendríamos con el aire acondicionado, podemos por ejemplo hacer este tipo de cosas:

1. **Controlar en encendido antes de llegar a casa**. Así cuando llegamos ya tenemos la casa fresquita. Para ello podemos utilizar un ESP8266 o un ESP32 en lugar de un Arduino Nano y conectarlo a la WiFi de casa.
2. **Parada controlada cuando el depósito de agua está lleno**. El aire acondicionado genera agua, todos lo sabemos, y muchos en lugar de tirarla por ahí, tenemos un recipiente donde dejamos que caiga el agua y lo vamos vaciando cada cierto tiempo. Que tal un sensor de llenado en el recipiente y que envíe un mensaje a nuestro ESP8266/ESP32 para que apague el aire acondicionado para que no derrame agua. Y de paso, nos envía un mensaje por Telegram....vamos esto es muy sencillo, no es ciencia ficción.
