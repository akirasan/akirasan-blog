---
layout: ../../layouts/post.astro
title: "Temporizador anticaries LED + Arduino"
pubDate: 2016-02-09
description: "Los expertos recomiendan un tiempo de cepillado de dos minutos. Pero realmente cuando vas a lavarte los dientes este tiempo es aproximado y"
author: "akirasan"
isPinned: false
excerpt: "Los expertos recomiendan un tiempo de cepillado de dos minutos. Pero realmente cuando vas a lavarte los dientes este tiempo es aproximado y"
image:
  src: "/content/images/2016/02/IMG_20160129_001400-1.jpg"
  alt: "Temporizador anticaries LED + Arduino"
tags: ["Arduino", "DIY"]
---

Los expertos recomiendan un tiempo de cepillado de dos minutos. Pero realmente cuando vas a lavarte los dientes este tiempo es aproximado y no sabes realmente cuanto tiempo has invertido.

Pues qué pasa si esto lo trasladamos al mundo infantil, pues que los niños hacen dos pasadas y listo, en 0,5seg ya han acabado de lavarse los dientes. Entonces, ¿cómo los mantienes dos minutos ahí con el cepillo?, pues la solución pasa por gamificar esta tarea. Aquí os dejo una solución fácil de implementar.

Ingredientes necesarios:

* 1 matriz de leds con controlador MAX7219
* 1 Arduino mini (en mi caso) o nano.
* 1 Interruptor
* 1 Cargador de móvil reciclado
* 1 Trozo de madera
* 1 Trozo de gomaeva

Las matrices leds son estas. Son 4 matrices de 8x8 que pueden ir conectadas entre ellas. De hecho estas ya vienen así montadas:

![Small](/content/images/2016/02/IMG_20160126_125358.jpg#small)  
![Small](/content/images/2016/02/IMG_20160126_125407.jpg#small)

La programación y pruebas con arduino definen el cableado, por eso no he puesto ningún pineado fijo.

![](/content/images/2016/02/IMG_20160129_001400.jpg)

Vamos a ver el código. Una gran parte del código son los diferentes dibujos que se muestran en cada matriz. Para ello se utilizan tablas de 8 lineas con 8 bits cada uno, vamos que si hay un 0, el led estará apagado y si hay un 1, se encenderá. Muy fácil, por ejemplo, el número 0 se "pinta" así:

```auto
byte n0[8] = {B00111100,
              B01100110,
              B01100110,
              B01100110,
              B01100110,
              B01100110,
              B01100110,
              B00111100
             };
```

Para el control de las matrices de led con el chip MAX7219 que llevan incorporado utilizamos la librería **LedControl.h**

```auto
#include "LedControl.h"

LedControl lc = LedControl(2, 4, 3, 4); /* DIN, CLK, CS, NUM_DISPLA */
```

Buen en la instancia de la variable **lc** indicamos en este orden el pin de Arduino que hemos utilizado para conectarlo `LedControl(2, 4, 3, 4);` viene siendo `LedControl(DIN, CLK, CS, NUMERO_DE_MATRICES_LED);`

Nos centramos en el `loop()`.

```auto
void loop() {

  if (inicio) {
    cuenta_atras();
    inicio = false;
    tiempo_15seg = millis();
    numero_avisos = 0;
  }
```

La variable booleana *inicio* nos marca precisamente el encendido del temporizador. Tomamos nota de los milisegundos ya que cada 15.000 (15 segundos) vamos a quitar una parte de la *caries*

En la siguiente parte, vamos a animar el cepillo hasta que no lleguemos al final del tiempo, los 2 minutos. Eso significa que tenemos que ejecutar 8 ciclos de 15 segundos (total de 2 minutos)

```auto
  if (!final) {
    cepilla();

    if (((millis() - tiempo_15seg) / 1000) >= 15) {
      aviso_15seg();
      numero_avisos++;
      tiempo_15seg = millis();
    }

    if (numero_avisos == 8) {
      fin();
      final = true;
    }
  }
}
```

Y de paso, vamos a aprovechar que son 8 ciclos, para que en cada uno de ellos se borre un línea del dibujo de la *caries*.

###### El programa en Arduino

Bueno todo el código lo podéis descargar desde aquí [lavatelosdientes.ino](http://akirasan.net/content/images/2016/02/lavatelosdientes.ino) (**haciendo botón derecho, guardar enlace como**).

###### Montaje final en madera

He utilizado una madera fina para usarla como soporte, se trata de medir, cortar y pegar :)

![Small](/content/images/2016/02/IMG_20160207_173135.jpg#small)  
![Small](/content/images/2016/02/IMG_20160207_180334.jpg#small)  
![Small](/content/images/2016/02/IMG_20160207_181335.jpg#small)  
![Small](/content/images/2016/02/IMG_20160207_181812.jpg#small)

Finalmente cubrimos la parte delantera con un poco de gomaeva y así queda montado cuando se usa.

![Normal](/content/images/2016/02/IMG_20160207_184515.jpg)

Os he dejado un vídeo de los 2 minutos reglamentarios por si no podéis montar el vuestro :) :

#### Actualización 13/06/2016

Cómo he creado un repositorio en [GitHub](https://github.com/akirasan), he dejado ahí el código Arduino para mayor comodidad :)

[MaxtriLed-dientes](https://github.com/akirasan/MaxtriLed-dientes)
