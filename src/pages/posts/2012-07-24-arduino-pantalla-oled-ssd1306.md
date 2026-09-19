---
layout: ../../layouts/post.astro
title: "Arduino + pantalla OLED (SSD1306)"
pubDate: 2012-07-24
description: "Recientemente me ha llegado una pantalla OLED de 128x64 pixels, preparada para arduino (permite la comunicación mediante ), comprada en ebay"
author: "akirasan"
isPinned: false
excerpt: "Recientemente me ha llegado una pantalla OLED de 128x64 pixels, preparada para arduino (permite la comunicación mediante ), comprada en ebay"
image:
  src: ""
  alt: "Arduino + pantalla OLED (SSD1306)"
tags: ["Arduino", "DIY"]
---

Recientemente me ha llegado una pantalla OLED de 128x64 pixels, preparada para arduino (permite la comunicación mediante [SPI](http://arduino.cc/en/Reference/SPI "http://arduino.cc/en/Reference/SPI")), comprada en ebay por unos 10€. Por si aún no puede ser mas sencilla la comunicación por hardware (gracias a estas placas ya ensambladas, sino a ver quien se traga la cantidad de información técnica para utilizar la pantalla: [http://www.gemmarduino.net/PM1/00100-101/SSD1306.pdf](http://www.gemmarduino.net/PM1/00100-101/SSD1306.pdf "http://www.gemmarduino.net/PM1/00100-101/SSD1306.pdf") ), existe una librería para arduino, que también nos simplifica mucho el código. Se trata de la librería [U8glib](http://code.google.com/p/u8glib/ "http://code.google.com/p/u8glib/") (*Universal Graphics Library for 8bits Embedded Systems*) que soporta una serie de dispositivos de este tipo, entre ellos el SSD1306 ([aquí la lista completa](http://code.google.com/p/u8glib/wiki/device "http://code.google.com/p/u8glib/wiki/device") y el constructor de la clase a utilizar).

Para empezar vamos a tener que descargar la librería U8glib y añadirla al directorio de libraries del IDE de arduino. Luego arrancamos el IDE y ya podremos utilizarla (esto es fácil y no voy a entrar en detalle).

Os dejo un simple *sketch* como ejemplo, que permite ver algunas de las funcionalidades de esta librería y de paso probar como funciona la pantalla OLED. Para mi ejemplo he tenido que utilizar la clase ***U8GLIB\_SSD1306\_128X64*** que utilizar comunicación SPI por software y no por hardware. Esto dependerá un poco del circuito que compréis.

```auto
#include <U8glib.h>

#define cs 2     // CS
#define a0 3     // DC
#define reset 4  // RST
#define sck 5    // D0
#define mosi 6   // D1

int x, y;

//SPI Comunicación por SW (sck, mosi, cs, a0 , reset)
U8GLIB_SSD1306_128X64 u8g(sck, mosi, cs, a0, reset);

void setup() {
  x = 50;
  y = 5;
}

void loop() {
  // picture loop
  u8g.firstPage();
  do {
   draw("Hello mundo!!!!");
   pie(x, y, "by akirasan 2k12");

   u8g.drawBox(10,22,20,30);

   u8g.drawCircle(50, 50, 14);

   u8g.drawFrame(110,30,15,10);

   u8g.drawPixel(14,23);
   u8g.drawPixel(24,43);
   u8g.drawPixel(125,53);
   u8g.drawPixel(0,0);
   u8g.drawPixel(127,0);
   u8g.drawPixel(127,63);
   u8g.drawPixel(0,63);

  } while( u8g.nextPage() );
  if (y <= 62) {
    y++;
  }

  // rebuild the picture after some delay
  delay(5);
}

void draw(char* text) {
  // graphic commands to redraw the complete screen should be placed here
  u8g.setFont(u8g_font_unifont);
  u8g.drawStr( 0, 20, text);
}

void pie(int x, int y, char* text) {
  // graphic commands to redraw the complete screen should be placed here
  u8g.setFont(u8g_font_04b_03);
  u8g.drawStr( x, y, text);
}
```

El resultado de este programita es el siguiente:

![](/content/images/2015/01/DSC_2984.JPG)

Muy fácil, verdad??,...pues existe otra librería llamada <http://code.google.com/p/m2tklib/> que soporta la U8glib y permite realizar GUI (graphical user interface), es decir, menús y control de entrada de datos para interactuar. Aún no la he probado, pero seguro que tiene que ser igualmente sencillo (un posible post en el futuro).

Existen otros tipos de pantalla OLED y evidentemente circuitos con pines diferentes. En este caso en concreto, voy a explicar cual es el pineado necesario y su relación con el constructor de la clase de la librería U8glib. Estos son los pines que trae este circuito:

![](/content/images/2015/01/DSC_2991.JPG)  
![](/content/images/2015/01/DSC_2990.JPG)

Esta es la relación de pines (OLED) y parámetros (U8glib) y pines digitales (arduino) (sin contar con los pines de alimentación GND y V (a +3.3V) ):

|  |  |  |
| --- | --- | --- |
| **PIN OLED** | **Parámetro clase** **U8GLIB\_SSD1306\_128X64** | **PIN arduino** |
| CS | cs | 2 |
| DC | a0 | 3 |
| RST | reset | 4 |
| D0 | sck | 5 |
| D1 | mosi | 6 |
| CS | cs | 2 |

PP: Me he pedido otra pantalla de color blanco,... :)
