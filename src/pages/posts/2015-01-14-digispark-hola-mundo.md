---
layout: ../../layouts/post.astro
title: "Digispark, \"hola mundo\""
pubDate: 2015-01-14
description: "es un microcontrolador basado en **ATtiny85** con una interface USB ya incorporada en placa. La codificación es similar a Arduino y se puede"
author: "akirasan"
isPinned: false
excerpt: "es un microcontrolador basado en **ATtiny85** con una interface USB ya incorporada en placa. La codificación es similar a Arduino y se puede"
image:
  src: "/content/images/2015/01/IMG_20150114_143830.jpg"
  alt: "Digispark, \"hola mundo\""
tags: []
---

[Digispark](http://digistump.com/category/1) es un microcontrolador basado en **ATtiny85** con una interface USB ya incorporada en placa. La codificación es similar a Arduino y se puede usar el IDE de desarrollo de Arduino.

Lo primero es bajarse he instalar el driver USB para que sea detectado por nuestro sistema. Podremos encontrar drivers para casi todos los sistemas (Linux, OSX y Windows):

<http://sourceforge.net/projects/digistump/files/>

Hay que descargar el fichero adecuado segun el sistema operativo que utiliceis y que tenga un nombre como este: *DigisparkArduino-Linux64-1.0.4-May.zip*. Una vez descomprimido encontraréis todo lo necesario: driver USB y IDE de Arduino compatible (versión para Windows):  
![](/content/images/2015/01/digispark_windows.JPG)  
Primero, evidentemente tenemos que instalar el driver y luego una vez nos reconozca el dispositivo Digispark y configurar el IDE de la siguiente forma, ya podremos comenzar a cargarle un programa de ejemplo.

Configuración del tipo de tarjeta. Seleccionamos **Digispark (Tiny Core)**  
![](/content/images/2015/01/digispark_ide_tarjeta.JPG)  
Configuración del puerto serie, variará en cada caso. En este ejemplo es el *COM27*  
![](/content/images/2015/01/digispark_ide_COM.JPG)  
Configuración del programador. Seleccionamos el **Digispark**  
![](/content/images/2015/01/digispark_ide_programador.JPG)  
Por el último el código fuente y cargamos el programa:  
![](/content/images/2015/01/digispark_ide_upload.JPG)

Aquí os dejo el fuente del programita que he creado. Muy sencillo y que únicamente genera un parpadeo cada segundo del LED incorporado. La única apreciación es ver el modelo de Digispark que usais y ver si en lugar del PIN 1 se utiliza el PIN 0 (cambiando el "1" por el "0" ya lo tendríais adaptado)

```auto
void setup() {                
	pinMode(1, OUTPUT); //LED en el Modelo A   
}

void loop() {
	digitalWrite(1, HIGH);   
    delay(75);               
	digitalWrite(1, LOW);   
    delay(75);               
    digitalWrite(1, HIGH);   
    delay(75);               

	digitalWrite(1, LOW);   
    delay(1000);            
  }
```
