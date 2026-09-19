---
layout: ../../layouts/post.astro
title: "IR + ESP8266 + MQTT + Node-RED = Aire Acondicionado domotizado"
pubDate: 2020-08-20
description: "Si ya puedes enviar comandos de infrarrojos para el aire acondicionado utilizando un Arduino, ahora toca darle una vuelta y conectarlo vía W"
author: "akirasan"
isPinned: false
excerpt: "Si ya puedes enviar comandos de infrarrojos para el aire acondicionado utilizando un Arduino, ahora toca darle una vuelta y conectarlo vía W"
image:
  src: "/content/images/2020/08/portada-1.jpg"
  alt: "IR + ESP8266 + MQTT + Node-RED = Aire Acondicionado domotizado"
tags: ["ESP8266", "DIY", "IoT"]
---

Si ya puedes enviar comandos de infrarrojos para el aire acondicionado utilizando un Arduino, ahora toca darle una vuelta y conectarlo vía WiFi, para ello utilizaremos un ESP8266.

Este proyecto de *domotización* de un split de aire acondicionado lo inicié en una entrada anterior que [podéis consultar aquí](http://akirasan.net/ir-arduino-control-del-aire-acondicionado/). La idea es poder controlar de forma remota, utilizando conexión WiFi, un split de aire acondicionado, independientemente de la marca y modelo.

## Hardware que vamos a utilizar

Vamos a utilizar un pequeño **ESP8266-01**, que para lo que necesitamos hacer es mas que suficiente para: emitir códigos mediante un LED infrarrojo, conectar a nuestra WiFi y a un servidor MQTT.

![](/content/images/2020/08/image-5.png)![](/content/images/2020/08/image-6.png)

Cómo estos módulos trabajan a 3.3V, voy a utilizar un pequeño convertidor **AMS1117 DC-DC de 5V a 3.3V**, que lo voy a conectar a un cable USB para que pueda ser utilizado por cualquier cargador y reaprovecharlo. Aquí busca la mejor forma de alimentarlo.

![](/content/images/2020/08/ams1117_small.png)

Y cómo ayuda visual al estado del módulo le he colocado un LED RGB WS2812b (Neopixel), porque cómo todos sabemos ***"donde hay un led, hay alegría"***. La verdad es que en estos dispositivos, **la ayuda visual** es de gran ayuda.

![](/content/images/2020/08/esp8266_ir_ws2812.jpg)

## El software para el ESP8266

Por suerte tenemos una librería llamada *IRremoteESP8266.h* que nos va a simplificar la tarea de utilizar un diodo LED de infrarrojos mediante un pequeño ESP8266. Si utilizas Platformio la puedes encontrar sin problema. Dejo por aquí [el link a repo Github de la librería](https://github.com/crankyoldgit/IRremoteESP8266?utm_source=platformio&utm_medium=piohome):

![](/content/images/2020/07/image-19.png)![](/content/images/2020/07/image-20.png)

También vamos a utilizar las librerías: ***ESP8266WiFi.h*** para conectarnos a la WiFi, ***PubSubClient.h*** para la mensajería MQTT y he utilizado ***Adafruit\_NeoPixel.h*** para el control del LED RGB.

Todo esto lo podrás ver en el programa que he creado para el proyecto. Código fuente que podrás encontrar [en mi repositorio de Github](https://github.com/akirasan/esp8266_aa_IR).

Cómo en todo programa de comunicación he dejado una sección para modificar las variables de configuración:

![](/content/images/2020/08/image-10.png)

Configura tu conexión WiFi (he dejado el método mas sencillo y rápido), dirección IP del servidor MQTT y su puerto (por defecto 1883). Y si no te gustan los topics de suscripción de la mensajería ahí los puedes modificar. Recuerda que si vas a utilizar mas de un nodo de este tipo, lo ideal es que cada uno genere su propia mensajería. Por ejemplo, si tienes varios splits a controlar, una propuesta sería:

* casa/aire/**habitacion**/estado
* casa/aire/**comedor**/estado
* casa/aire/**cocina**/estado

Otro punto importante en el programa, es donde configuramos los códigos IR en formato RAW que hemos capturado previamente con **IRrecvDumpV2.ino** y que nos permite clonar los códigos que envía el mando remoto.

![](/content/images/2020/08/image-11.png)

## Conexión entre IR y el ESP8266

Buscando información sobre proyectos similares con un ESP8266 y un led IR, he encontrado varios donde describen una conexión mediante un transistor tipo ****N222, 2N3904,BC547**.**

<https://www.instructables.com/id/Universal-Remote-Using-ESP8266Wifi-Controlled/>

![](/content/images/2020/08/image-3.png)

<http://blog.2the.top/2017/08/22/Diy-Ir-Remote-With-ESP8266-01/>

![](/content/images/2020/08/image-4.png)

En mis pruebas iniciales no tenía ningún problema con conectar el LED IR emisor a un pin de Arduino o al GPIO2 del ESP8266-01 (cuando estaba conectado con el FTDI). Pero cuando monté el prototipo completo en una *breadboard* entendí porque se proponía utilizar un transistor para controlar el LED IR

![](/content/images/2020/08/esp8266_ftdi.jpg)

### El problema de conectar un LED al pin GPIO2 de un ESP8266-01

Aquí invertí bastante tiempo en entender que pasaba: una vez programado el ESP8266 lo conectaba a la *breadboard* de pruebas y cuando conectaba alimentación el ESP8266 no arrancaba ni ejecutaba el *firmware* (el led azul se quedaba fijo). Haciendo pruebas, me di cuenta que cuando quitaba el LED IR que estaba conectado al GPIO2, el ESP8266 funcionaba sin problema y arrancaba. Luego le volvía a conectar el LED IR al GPIO2 (una vez había arrancado) y todo funcionaba.

Resulta que los pines GPIO0 y GPIO2 del ESP8266 se utilizan para establecer el modo boot del módulo, es decir, si queremos arrancar en modo normal o en flash para actualizar firmware. Al tener un led conectado en el GPIO2 durante ese proceso de boot, el ESP8266 entendía que tenía que entrar en modo flash y se quedaba ahí.

Por esta razón, en los proyectos que encontré utilizaban un transistor cómo "*interruptor*" para controlar el LED IR, y de esa forma el GPIO2 no se veía afectado para el boot del ESP8266.

Yo cómo estoy muy loco y soy un maker de andar por casa, he buscado otra opción: **utilizar el GPIO3**, el que usa el ESP8266 cómo puerto RX, total no se usa durante el boot ni para recibir UART durante el funcionamiento del dispositivo.

![](/content/images/2020/08/image-8.png)

Por lo tanto tendremos el GPIO0 para controlar el LED RGB WS2812B y el GPIO3 (URXD/RX) para el LED IR.

## Servidor MQTT y Node-RED

**No es parte de este tutorial explicar la configuración y uso de Node-RED y Mosquitto.**

Cómo servidor MQTT he utilizado [Mosquitto](https://mosquitto.org/) y cómo sistema de programación de flujos y eventos voy a utilizar Node-RED.

Ahora hacemos un pequeño flujo en Node-RED para probar la comunicación y todo el circuito:

![](/content/images/2020/08/image.png)

Vamos a enviar topics con valores fijos que luego nuestro ESP8266 recibirá, ya que está suscrito a estos mensajes.

![](/content/images/2020/08/image-1.png)![](/content/images/2020/08/image-2.png)

Una vez que tenemos nuestro ESP8266 conectado a nuestra WiFi y que además es capaz de publicar y suscribir a nuestro servidor MQTT, las posibilidades de integración se disparan y ya podemos, por ejemplo, desde Node-RED generar una planificación para que se apague o encienda según un horario, condiciones de temperatura exterior, etc.

Pero si quieres controlarlo desde de tu móvil, una opción es utilizar una aplicación cómo [IOTMQTTPanel](https://play.google.com/store/apps/details?id=snr.lab.iotmqttpanel.prod) que te permite conectarte a tu servidor MQTT y generar un layout interactivo que envía mensajes o se suscribe a publicaciones. La creación de estos layouts interactivos son muy sencillos y fáciles de implementar. Aquí os dejo un ejemplo de mi proyecto:

![](/content/images/2020/08/image-13.png)

Por ejemplo, para el botón de "**AUTO 27ºC**" tengo definido esto (además tiene un icono muy representativo, todo esto viene en la propia App):

![](/content/images/2020/08/image-14.png)

Además si tenemos nuestro servidor MQTT **debidamente protegido** y abierto hacia el exterior, esto lo podríamos hacer desde fuera de nuestra red interna.

### Resultado final (WIP)

Aún sigo trabajando en el proyecto. Queda dejarlo un poquito mas bonito de forma externa, tal vez con una carcasa diseñada e impresa en 3D,...pero por ahora es 100% funcional y hace su función. Está claro que para dejarlo en medio del comedor no tiene buena pinta, pero el mío por suerte, no se ve ;)

![](/content/images/2020/08/esp8288_ir_ws2812_proto_v1.jpg)

---

**Artículo anterior:** [https://akirasan.net/ir-arduino-control-del-aire-acondicionado/](http://akirasan.net/ir-arduino-control-del-aire-acondicionado/)

**Código en Github:** <https://github.com/akirasan/esp8266_aa_IR>
