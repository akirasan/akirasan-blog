---
layout: ../../layouts/post.astro
title: "Controlar MAX7219 LED Matrix con ESP8266-12E"
pubDate: 2016-07-22
description: "Utilizando el IDE de Arduino vamos a programar la placa de desarrollo **LoLin**, que lleva un módulo WiFi **ESP8266-12E** integrado (ideal p"
author: "akirasan"
isPinned: false
excerpt: "Utilizando el IDE de Arduino vamos a programar la placa de desarrollo **LoLin**, que lleva un módulo WiFi **ESP8266-12E** integrado (ideal p"
image:
  src: "/content/images/2016/07/IMG_20160722_011916_20160722_014259.jpg"
  alt: "Controlar MAX7219 LED Matrix con ESP8266-12E"
tags: ["ESP8266", "Arduino"]
---

Utilizando el IDE de Arduino vamos a programar la placa de desarrollo **LoLin**, que lleva un módulo WiFi **ESP8266-12E** integrado (ideal para proyectos *IoT*), para controlar uno o varios módulos de display LED tipo matriz con el chip **MAX7219**.

Pero para empezar vamos a tener que instalar primero la librería [MAX7219LedMatrix](https://github.com/squix78/MAX7219LedMatrix) especialmente desarrollada para ser utilizada con el ESP8266.

La instalación es sencilla: Descargamos la librería en formato ZIP desde el GitHub <https://github.com/squix78/MAX7219LedMatrix> y la añadimos a nuestro IDE:

![](/content/images/2016/07/Selecci-n_006.png)

Esta librería por defecto ya tiene definido una serie de pines que tenemos que respetar (a parte de la alimentación VCC y GND):

* **DIN** al pin **GPIO13 o D7** en la placa *LoLin*
* **CLK** al pin **GPIO14 o D5** en la placa *LoLin*

Para no perderse mejor seguir este esquema cómo referencia de la placa de desarrollo LoLin:

![](/content/images/2016/07/esp8266-pinout.jpg)

Luego hay una serie de parámetros que tenemos que definir desde el código:

```auto
#define NUMBER_OF_DEVICES 3
#define CS_PIN 2
```

* `NUMBER_OF_DEVICES`: Indicaremos cuantas matrices LED vamos a utilizar, en mi ejemplo voy a utilizar 3
* `CS_PIN`: A diferencia de los pines *DIN* y *CLK* el *CS* puede ser definido donde queramos, yo he escogido el "pin 2", que equivale al **GPIO2 o D4** en la placa *LoLin*

Detalle de cómo lo he conectado a mi placa LoLin (D4, D5, D7, G(*GND*), 3V):

![](/content/images/2016/07/IMG_20160722_012033_20160722_014300.jpg)

El código de ejemplo que os propongo es bastante sencillo: una serie de textos que irán de derecha a izquierda.

```auto
#include <SPI.h>
#include "LedMatrix.h"

#define NUMBER_OF_DEVICES 3
#define CS_PIN 2
LedMatrix ledMatrix = LedMatrix(NUMBER_OF_DEVICES, CS_PIN);
int x = 0;
  
void setup() {
  ledMatrix.init();

  ledMatrix.setText("akirasan");
  ledMatrix.setNextText(";)");
}

void loop() {

  ledMatrix.clear();
  ledMatrix.scrollTextLeft();
  ledMatrix.drawText();
  ledMatrix.commit();
  delay(50);
  x=x+1;
  if (x == 200) {
     ledMatrix.setNextText("MAX7219 + ESP8266"); 
  }
}
```

El código cómo siempre, está disponible en [mi canal de GitHub](https://github.com/akirasan/ledmatrix_esp8266).

Cómo he comentado he utilizado tres módulos de matrices LED con el MAX7219 enlazados secuencialmente:

![](/content/images/2016/07/IMG_20160722_012124_20160722_014302.jpg)

La librería tiene una serie de métodos sobre la clase `LedMatrix` que nos permite hacer bastantes cosas, veamos por ejemplo algunos de los métodos que hemos utilizado en el código anterior:

* `setText`: Especifica el texto actual.
* `setNextText`: Especifica el siguiente texto a mostrar tras el scroll del texto actual.
* `clear`: Borra el buffer.
* `commit`: Vuelca todo el buffer a los displays.
* `drawText`: Muestra el texto actual en el offset actual.

A parte la librería también tiene otros métodos interesantes cómo:

* `scrollTextRight()`: Realiza el scroll hacia el lado derecho.
* `setPixel(byte x, byte y)`: Activa el pixel de la posición x,y.
* `setIntensity(byte intensity)`: Define la intensidad de iluminación de todos los displays. El valor va de 0 a 15.

Veamos cómo funciona con vídeo de ejemplo, donde utilizo tres displays:

Bien, pues ahora que tenemos un módulo ESP8266 conectado a una serie de displays es hora de pensar en alguna aplicación práctica, cómo por ejemplo conectar el módulo ESP8266 a nuestra WiFi y enviarle mensajes para que los muestre por los displays.
