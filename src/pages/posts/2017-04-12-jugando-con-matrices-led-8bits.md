---
layout: ../../layouts/post.astro
title: "Jugando con matrices led 8bits"
pubDate: 2017-04-12
description: "Una forma interesante de hacer llegar el concepto de **Fabricación Digital** entre los más peques, es haciéndoles entender que cualquier cos"
author: "akirasan"
isPinned: false
excerpt: "Una forma interesante de hacer llegar el concepto de **Fabricación Digital** entre los más peques, es haciéndoles entender que cualquier cos"
image:
  src: "/content/images/2017/04/IMG_20170317_193150-1.jpg"
  alt: "Jugando con matrices led 8bits"
tags: ["Arduino", "DIY"]
---

Una forma interesante de hacer llegar el concepto de **Fabricación Digital** entre los más peques, es haciéndoles entender que cualquier cosa que puedan pensar y trasladar a un diseño, luego lo pueden implementar utilizando herramientas tecnológicas. Además ayuda a entender a los padres cómo funcionan las cosas, construyendo pequeños proyectos.

Un proyecto que puede ser interesante para comenzar a conocer cómo funcionan esos carteles luminosos formados por pequeños leds, es el utilizar y entender a pequeña escala cómo funcionan, por ejemplo con una matriz de leds de 8x8 y con el controlador de displays MAX7219. Vamos algo sencillo de encontrar en formato módulo para Arduino.

![](/content/images/2017/04/IMG_20170317_193150.jpg)

El primer paso sería hacer un diseño. Para ello podemos utilizar una hoja de papel cuadriculada y enmarcar 8x8 casillas. Esto representará nuestra matriz de led digital, que también es de 8x8.

![](/content/images/2017/04/IMG_20170412_211843.jpg)

###### El hardware

Para este ejemplo he utilizado un pequeño arduino mini, pero evidentemente cualquier otro tipo es viable.

![](/content/images/2017/04/IMG_20170412_211924.jpg)

Y por otro lado un módulo MAX7219 que lleva una matriz de leds de 8x8  
![](/content/images/2017/04/IMG_20170412_211857.jpg)

El tema de las conexiones es libre, siempre y cuando luego cuando lleguemos a la parte del software sea coherente. Yo por ejemplo he utilizado este pineado (luego lo veréis definido en el programa):

pin 12 arduino --> DIN  
pin 11 arduino --> CLK  
pin 10 arduino --> CS  
pin Vcc arduino --> VCC  
ping GND arduino --> GND

###### A por la programación

Una vez tenemos nuestro diseño y el hardware mas o menos listo, tenemos que conseguir traducir nuestro diseño en papel a código, para yo he utilizado la librería [LedControl.h](http://wayoda.github.io/LedControl/). Veamos el código por partes:

###### Definición de la matriz de leds

Primero definimos la librería y revisamos que el cableado se corresponda con lo que vamos a definir, en este caso vamos a utilizar los pines 12, 11 y 10 de nuestro Arduino, que irán conectados a DIN, CLK y CS respectivamente, que trae el módulo matriz leds. A parte también le conectaremos los de alimentación VCC y GND. En total vamos a necesitar cinco cables.

`#include "LedControl.h"  
/*  
Definimos los pines que utilizará LedControl  
pin 12 conectado para DataIn/DIn  
pin 11 conectado para CLK  
pin 10 conectado para LOAD  
*/  
/*  
Creamos el objeto MatrizLED como LedControl utilizando la definicion:  
LedControl(pin DataIn, pin CLK, pin LOAD, número de matrices led);  
*/  
LedControl MatrizLED=LedControl(12,11,10,1);`

###### setup()

Ahora pasamos a la parte del código donde vamos a preparar nuestro elemento `MatrizLED`. Sólo tenemos que tener en cuenta que, aunque hayamos definida una única matriz de leds, podríamos utilizar varias y la numeración cuando queremos trabajar con ellas comienza de [0..n], ya que internamente la librería la trata como una array. Por lo tanto si decimos que tenemos una matriz, nos referimos a ella con el 0. Si tenemos dos, la primera es 0 y la segunda es 1. Si tenemos tres: 0,1,2,…y así sucesivamente.

El código está suficientemente documentado para ver que hace cada línea en este caso:

`void setup() {  
// Iniciamos nuestra matriz de leds  
// Tened el cuenta que la primera matriz se referencia con "0"  
MatrizLED.shutdown(0,false); // apagamos todos los leds de la matriz "0"  
MatrizLED.setIntensity(0,8); // definimos la intensidad "8" de la matriz "0"  
MatrizLED.clearDisplay(0); // limpiamos cualquier información previa de la matriz "0"  
}`

###### El loop() y nuestra función de dibujo

Y por último vamos a definir una rutina que llamaremos `DibujoMatriz`, que va a contener el código para iluminar los leds que queremos:

`void DibujoMatriz() {  
byte leds[8]={  
B00000000,  
B01111110,  
B00000010,  
B01111110,  
B01000000,  
B01111110,  
B00000010,  
B01111110  
};  
for(int fila=0;fila<8;fila++) {  
MatrizLED.setRow(0,fila,leds[fila]);  
}  
}  
void loop() {  
DibujoMatriz();  
}`

Para entender como pasar nuestro dibujo de papel a la matriz de `leds[8]`. Es una array de bytes (8 bits) de 8 elementos,…curioso eh?, tenemos 8x8, justo lo que mide nuestra matriz de leds: 8x8 leds.

*¿Porque hemos utilizado esta definición?*, pues porque la librería LedControl tiene el método `setRow(0,fila,leds[fila]);` para nuestro elemento `MatrizLED` que pasando el número de fila y un número entre 0..255, es decir 1 byte que son 8bits. Es decir, deberíamos enviar un numero entre 0 y 255 para encender o apagar uno de los leds, por ejemplo:

Si pasamos el número 1 (dentro del rango de bytes de 0..255) se transforma binario a 00000001. Si utilizamos el método setRow(0,3,1) (0: número de display, 3: fila y 1 valor byte) tendríamos este resultado:

![](/content/images/2017/04/max7219_ledmatriz.jpg)

Cómo tener que traducir de binario a decimal es un poco rollo, existe la posibilidad de indicar al compilador de Arduino que el número está en formato binario, por esa razón los valores de la array son `B00000000`, en este caso el cero. Esto nos simplifica mucho, porque si ponemos en formato matriz, tenemos visualmente la matriz de leds tal cual la queremos:

![](/content/images/2017/04/max7219_ledmatriz_2.jpg)

Espero que haya quedado mas o menos claro con el ejemplo (menuda currada que me he pegado con el dibujo!!!).

En fin, una vez tenemos nuestro dibujo en papel trasladarlo a digital es sencillo, solamente tenemos que transformar los cuadrados en blanco por 0 y los que hemos pintado por 1, en la matriz de leds, encendiendo y apagando el led.
