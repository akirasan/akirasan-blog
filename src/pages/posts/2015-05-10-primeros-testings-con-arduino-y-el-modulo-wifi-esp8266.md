---
layout: ../../layouts/post.astro
title: "Primeros testings con Arduino y el módulo wifi ESP8266"
pubDate: 2015-05-10
description: "Por fin ya me he montado una placa para testear y prototipar proyectos con Arduino y el módulo wifi ESP8266-01. La idea de esta placa es pod"
author: "akirasan"
isPinned: false
excerpt: "Por fin ya me he montado una placa para testear y prototipar proyectos con Arduino y el módulo wifi ESP8266-01. La idea de esta placa es pod"
image:
  src: ""
  alt: "Primeros testings con Arduino y el módulo wifi ESP8266"
tags: ["Arduino", "DIY", "IoT", "ESP8266"]
---

Por fin ya me he montado una placa para testear y prototipar proyectos con Arduino y el módulo wifi ESP8266-01. La idea de esta placa es poder programar y probar la solución antes de pasar al diseño final (módulo FTDI USB para la comunicación y programación del arduino).

![](/content/images/2015/05/Selecci-n_034.jpg)  
![](/content/images/2015/05/Selecci-n_035.jpg)

Como se puede ver he utilizado un [Deek-Robot Pro Mini](http://akirasan.net/deek-robot-pro-mini-arduino-clon/), un clon de Arduino Pro Mini que ya habíamos hablado hace unos días y un regulador de voltaje de 5v a 3.3v (la tensión que acepta el ESP8266). Todo el circuito (incluido el arduino) está alimentado a 3.3v. Por cierto, si queréis ver como conectar un Deek Robot con el módulo FTDI USB para subirle un sketch desde el IDE de Arduino, podéis consultar [el post anterior](http://akirasan.net/deek-robot-pro-mini-arduino-clon/).

Hagamos un pequeño inciso aquí,...el módulo reductor de voltaje es un **Step-Down Power Supply Module AMS1117-3.3 LDO 800mA**, que permite bajar un tensión DC de entrada entre 4.5v-7v a 3.3V de una forma cómoda (no lo voy a negar, me estuve peleando montando un [LM317](http://es.wikipedia.org/wiki/LM317), con sus condensadores y resistencias ajustadas a lo que necesitaba,...pero después de ver lo pequeño y simple que es éste módulo, ¿quién quiere liarse!!?). ¿El precio? muy barato, solo 1,35€ (transporte incluido desde China y comprado en eBay). Con los 800mA de la salida tiene suficiente fuerza para alimentar el arduino y el módulo wifi sin problemas.

###### Consumo eléctrico del módulo

En la [wiki de iteadstudio](http://wiki.iteadstudio.com/ESP8266_Serial_WIFI_Module), he encontrado información interensan y sobretodo algo que preocupa a la hora de desarrollar nuestros sensores remotos: el consumo de batería ¿cuanto nos va a durar sin cambiar la batería?. La tabla con algunas pruebas y estados que nos ayudarán a optimizar la batería entendiendo el consumo del envío/recepción de los datos:

|  |  |  |  |  |
| --- | --- | --- | --- | --- |
| **Mode** | **Min** | **Typ** | **Max** | **Unit** |
| Transmit 802.11b, CCK 1Mbps, POUT=+19.5dBm |  | 215 |  | mA |
| Transmit 802.11b, CCK 11Mbps, POUT=+18.5dBm |  | 197 |  | mA |
| Transmit 802.11g, OFDM 54Mbps, POUT =+16dBm |  | 145 |  | mA |
| Transmit 802.11n, MCS7, POUT=+14dBm |  | 135 |  | mA |
| Receive 802.11b, packet length=1024 byte, -80dBm |  | 60 |  | mA |
| Receive 802.11g, packet length=1024 byte, -70dBm |  | 60 |  | mA |
| Receive 802.11n, packet length=1024 byte, -65dBm |  | 62 |  | mA |
| Standby |  | 0.9 |  | mA |
| Deep sleep |  | 10 |  | uA |
| Power save mode DTIM 1 |  | 1.2 |  | mA |
| Power save mode DTIM 3 |  | 0.86 |  | mA |
| Total shutdown |  | 0.5 |  | uA |

###### En resumen:

* Es preferible utilizar una alimentación a 3.3v para todo el circuito. Si no puedes, recuerda que el módulo wifi ESP8266 trabaja a 3.3v, si utilizas un arduino a 5v tendrás que tener en cuenta varias cosas: la tensión del pin Tx del arduino a 5v se tiene que bajar a 3.3v para el pin de entrada Rx del ESP8266 (tendrás que poner un divisor de voltaje). El módulo requiere unos 300mAh para transmitir, necesitarás algún condensador de unos 220uF para darle soporte.
* La fuente de alimentación es muy importante, debe ser constante y a 3.3v, sino obtendrás en la salida del módulo wifi múltiples errores.

Algunas reseñas:

<http://arduino-er.blogspot.com.es/2015/02/first-try-esp8266-low-cost-wifi-module.html>

<http://iot-playground.com/2-uncategorised/13-esp8266-wifi-module-arduino-connection>
