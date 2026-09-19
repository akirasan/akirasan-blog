---
layout: ../../layouts/post.astro
title: "Proyecto GPS+D90+Arduino (Parte 1)"
pubDate: 2012-01-13
description: "He comenzado un proyecto (y voy a tratar de acabar,...) en el cual, la idea básica es: conectar un  vía bluetooth a un módulo Arduino Nano v"
author: "akirasan"
isPinned: false
excerpt: "He comenzado un proyecto (y voy a tratar de acabar,...) en el cual, la idea básica es: conectar un  vía bluetooth a un módulo Arduino Nano v"
image:
  src: ""
  alt: "Proyecto GPS+D90+Arduino (Parte 1)"
tags: []
---

He comenzado un proyecto (y voy a tratar de acabar,...) en el cual, la idea básica es: conectar un [GPS Holux M241](http://www.holux.com/JCore/en/products/products_content.jsp?pno=341 "http://www.holux.com/JCore/en/products/products_content.jsp?pno=341") vía bluetooth a un módulo Arduino Nano v3.0 que está conectado por cable al conector de GPS de mi Nikon D90, con la finalidad de que la cámara geoposicione de forma automática las fotos. Vamos, que guarde en el Exif, info del punto donde fue tomada la foto. Este es el concepto.

En esta primera parte he conectado el Arduino por cable a la Nikon D90 y he simulado la entrada de datos del GPS en formato NMEA. Para ello hará falta: un Arduino (en mi caso un Nano v3.0, pero cualquier versión vale), un pequeño programa (*sketch*) para que Arduino simule la salida de datos y un conector para la entrada GPS de la cámara (ahora explicaré como conseguirlo).

El conector GPS para la Nikon D90: Se puede conseguir muy barato en eBay (por unos 5$-6$) un disparador remoto por cable (modelo **MC-DC2**). De este cable únicamente nos servirá el conector, el cual hay que volverlo a pinear correctamente. [En esta web tenéis el pineado necesario](http://grink.com/2010/12/05/nikon-d90-homemade-gps/ "http://grink.com/2010/12/05/nikon-d90-homemade-gps/") (es de donde he sacado algo de información). Esta es la pinta que tiene el mío (evidentemente sin el envoltorio):

![conector](http://static.zooomr.com/images/10147952_7e8a33b8a2.jpg)

Bien, una vez pineado el conector de forma correcta, hay que centrarse en el Arduino. Os dejo el programa que he generado como ejemplo para simular la entrada de datos GPS a la cámara:

![](http://static.zooomr.com/images/10147953_de94f707cf.jpg)
> ```auto
>  #include <SoftwareSerial.h>
>  */
>  GPS test D90
>  SoftwareSerial(rxPin, txPin)
>  rxPin = 2
>  txPin = 3
>  */
>  SoftwareSerial mySerial(2, 3);
>  void setup()
>  {
>  mySerial.begin(4800);
>  pinMode(2,INPUT);
>  pinMode(3,OUTPUT);
>  pinMode(13,OUTPUT);
>  }
>  void loop()
>  {
>  digitalWrite(13,HIGH);
>  mySerial.println("$GPGGA,154654,4428.2011,N,00440.5161,W,0,00,,-00044.7,M,051.6,M,,6B");
>  mySerial.println("$GPGSA,A,1,,,,,,,,,,,,,,,1E");
>  mySerial.println("$GPGSV,3,1,10,02,50,290,00");
>  mySerial.println("$GPGGA,154655,4328.1874,N,00340.5185,W,1,03,08.5,-00044.7,M,051.6,M,,*79");
>  mySerial.println("$GPGSA,A,2,13,23,25,,,,,,,,,,08.5,08.5,00.9*0E");
>  mySerial.println("$GPGSV,3,1,10,02,50,290,26,04,60,210,26,08,33,173,29,10,21,296,00*7E");
>  mySerial.println("$GPGSV,3,2,10,13,58,044,34,16,03,035,00,20,02,109,00,23,26,057,34*7B");
>  mySerial.println("$GPGSV,3,3,10,25,24,045,35,27,56,145,27,,,,,,,,*7D");
>  mySerial.println("$GPRMC,154655,A,4428.1874,N,00440.5185,W,000.7,000.0,050407,,,A*6C");
>  digitalWrite(13,LOW);
>  }
> ```

Ahora ya solo queda conectar el cable a la salida del Arduino y a la entrada GPS de la D90, dar corriente al Arduino y encender la cámara y verificar que correctamente está recibiendo los datos:
![D90+GPS+Arduino](http://static.zooomr.com/images/10147951_6da4397cfc.jpg)

Si disparas alguna foto y consulta sus datos, verás que tiene la información del GPS.

Bueno hasta aquí la primera parte. Estoy a la espera de que me llegue el módulo bluetooth para pincharlo a la placa Arduino y ver de emparejar el GPS Holux (aunque evidentemente se podría utilizar un movil con GPS, porque no?)
