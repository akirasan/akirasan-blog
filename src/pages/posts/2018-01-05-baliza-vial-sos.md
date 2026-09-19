---
layout: ../../layouts/post.astro
title: "Baliza vial S.O.S. #OpenSource"
pubDate: 2018-01-05
description: "Los que solemos ir en moto, normalmente no llevamos un triángulo para señalizar una posible avería en la vía, de echo no llevamos nada. Diar"
author: "akirasan"
isPinned: false
excerpt: "Los que solemos ir en moto, normalmente no llevamos un triángulo para señalizar una posible avería en la vía, de echo no llevamos nada. Diar"
image:
  src: "/content/images/2018/01/upload-1514601601.jpg"
  alt: "Baliza vial S.O.S. #OpenSource"
tags: []
---

Los que solemos ir en moto, normalmente no llevamos un triángulo para señalizar una posible avería en la vía, de echo no llevamos nada. Diariamente voy en moto a trabajar y alguna vez he visto algún motorista esperando asistencia mecánica, y sin señalar su posición...todo un peligro. Además si esto pasa en el interior de un túnel o de noche mucho peor, porque no se ve absolutamente nada.

Donde trabajo hay una buena práctica antes de iniciar una reunión: se trata de hacer un contacto de seguridad. En uno de esos intercambios, alguien enseñó un producto comercial llamado [Help Flash](http://help-flash.com/). La idea me gustó y he creado una versión OpenSource DIY.

### Material necesario

* Arduino Mini Pro (versión 5V)
* Batería 9V 6LR61
* Condensador 1000uF
* Tira leds WS2812b de 1m, 60 leds/m y IP67 (solo necesitaremos un trozo con 14 leds)
* 2 Imanes de neodiomio: díametro 12mm x 3mm ancho
* 1 Interruptor (SPDT) de 8mm diametro
* 1 Pequeña placa PCB perforada de 30mmx36mm
* 1 Pila de 9v tipo 6LR61
* 1 Conector alimentación para pila 9v 6LR61
* 4 Tornillos M3 de 10mm
* 3 Piezas impresas en 3D
* (*opcional*) Mini push-button
* (*opcional*) Resistencia 10Kohm

![Selecci-n_067](/content/images/2018/01/Selecci-n_067.jpg)

#### Conexión de los elementos

La conexión de los elementos es relativamente sencilla: Primero preparamos nuestro Arduino Mini Pro con los pines y la placa PCB perforada.

![](/content/images/2017/12/upload-1514601536.jpg)  
![](/content/images/2017/12/upload-1514601580.jpg)

Podemos testear todos los componentes en una protoboard para ver que todo funciona correctamente según el esquema:

![Selecci-n_066](/content/images/2018/01/Selecci-n_066.jpg)  
![](/content/images/2017/12/upload-1514601556.jpg)

La **tira de LEDs WS2812b está cortada con 14 LEDs**, este tamaño es el adecuado para dar una vuelta completa y justa al diseño de la baliza S.O.S. Un extremo de la tira de LEDs se sueldan tres cables correspondientes a los pines GND, DIn y VCC y en el otro extremo se deja abierto.

Yo he optado por conexiones tipo *dupont* para todos los componentes: tira de leds, interruptor y push button, así en caso de cambio de algún elemento no necesito estar desoldando.

Esta sería **la versión sencilla**, la que no lleva el mini-pulsador que permite cambiar el modo de funcionamiento:

![](/content/images/2017/12/upload-1514601587.jpg)  
![](/content/images/2017/12/upload-1514601594.jpg)

Y **la versión con push button (pulsador)** quedaría como esto:

![](/content/images/2018/01/upload-1515045489.jpg)  
![](/content/images/2018/01/upload-1515045482.jpg)

Pero en un alarde de aprender algo mas, me he liado con [KiCAD](http://kicad-pcb.org/) y he diseñado una PCB para poder integrar estos componentes de una forma sencilla. Aquí os dejo el esquema y el resultado de la PCB.

![esquemaKiCAD](/content/images/2018/01/esquemaKiCAD.jpg)  
![PCB_KiCAD](/content/images/2018/01/PCB_KiCAD.jpg)

***Actualizado 11/01/2018*** Pues hoy mismo he recibido el diseño de la PCB (mi primera PCB!!!). A pesar de que es un diseño sencillo, estoy la mar de contento. Venga unas fotos del proceso de montaje.

![](/content/images/2018/01/upload-1515625802.jpg)

![](/content/images/2018/01/upload-1515625811.jpg)

![](/content/images/2018/01/upload-1515625817.jpg)

![](/content/images/2018/01/upload-1515625825.jpg)

![](/content/images/2018/01/upload-1515625831.jpg)

Y por supuesto montamos el interruptor conectado a la batería 9V.

![](/content/images/2017/12/upload-1514601565.jpg)

#### Impresión 3D

La baliza se imprime en tres piezas por separado y sin material de soporte.

![baliza-sos-3D](/content/images/2018/01/baliza-sos-3D.jpg)

**Pieza inferior**  
![parte-inferior](/content/images/2018/01/parte-inferior.jpg)

**Pieza superior**, que podemos girar 180 grados para imprimirla sin soportes:  
![parte-superior](/content/images/2018/01/parte-superior.jpg)

**Tapa superior**  
![tapa-superior](/content/images/2018/01/tapa-superior.jpg)

La pieza inferior y superior se unen mediante pegamento, lo veremos en el siguiente apartado de *montaje*. Mientras que la tapa va atornillada.

![union-parte-inf_sup](/content/images/2018/01/union-parte-inf_sup.jpg)

#### Montaje de la baliza

Pegamos la parte inferior y la parte superior mediante pegamento. Para ello utilizamos las guias que van insertadas de la pieza superior contra la inferior.

![](/content/images/2017/12/upload-1514601409.jpg)  
![](/content/images/2017/12/upload-1514601435.jpg)

También podemos poner un poco de pegamento en la unión de las dos piezas. Aquí es importante alinear el orificio que podemos ver en la foto. Este orificio rectangular (en esta foto no tiene la parte frontal) es para introducir los cables de un extremo de la tira de LEDs.

![](/content/images/2017/12/upload-1514601462.jpg)

Ahora en el fondo podemos pegar (también con pegamento) los imanes de neodimio (12mm de diámetro x 3mm de ancho). Aquí aconsejo poner debajo algo de metal, así la propia atracción ayuda a fijar la pieza sin que se mueva (y por cierto hacer una prueba antes para orientar el imán).

![](/content/images/2017/12/upload-1514601520.jpg)

#### Programación de Arduino Pro Mini

La verdad es que no voy a entrar en detalle del software de arduino, no es el objetivo del post y el programa es bastante sencillo. Solo recordad:

* Para programar un Arduino Pro Mini necesitas un FTDI.
* El programa funciona tanto con la versión con o sin push button.
* Los pines utilizados son:
  + **D2**: para comunicación con la tira de LEDs.
  + **D3**: para la lectura del push button.

Si optas por la opción del pulsador que permite cambiar de modos, has de saber que está programado para que cambie de forma cíclica a cada pulsación. Estos son los modos en los que cambia:

1. Flash amarillo en varios ángulos por secciones de LEDs.
2. Luz blanca fija (función linterna con una sección de LEDs).
3. Luz amarilla rotatoria.

Si por el contrario no le pones el pulsador, no pasa nada, simplemente se quedará en el primer modo: Flash amarillo en varios ángulos. Más que suficiente para señalizar una situación de emergencia.

#### Para acabar...algunos consejos

Para acabar, a parte de meterlo todo dentro de la baliza y ponerle la tapa con cuatro tornillos:

La batería se acopla muy bien en horizontal y se queda fijada al suelo por los imanes, así que no irá dando tumbos por ahí.

La electrónica (shield PCB con el Arduino Pro Mini) queda bastante justo, así que tampoco se mueve mucho. En caso de que tengáis problemas, se puede poner un poco de cola caliente.

![](/content/images/2017/12/upload-1514601601.jpg)

La tira de LEDs también va pegada a la baliza con un poco de pegamento. Y la entrada que los cables, desde el exterior al interior de la baliza es aconsejable sellarla mediante cola caliente.

El peso general de la baliza ya montada son de unos 142g, así que bastante portable.

![](/content/images/2018/01/upload-1515045514.jpg)

![](/content/images/2018/01/upload-1515625905.jpg)

Como lo he desarrollado como un proyecto OpenSource, está abierto a mejoras, tanto de la electrónica, como del diseño. Así que todos los ficheros fuentes (modelos .STL, diseño 3D, código Arduino y diseño PCB) los podéis encontrar en mi GitHub <https://github.com/akirasan/Baliza-SOS>

Cualquier duda o comentario podéis [encontrarme en Twitter ;)](https://twitter.com/akirasan)

Y para finalizar un vídeo DEMO
