---
layout: ../../layouts/post.astro
title: "KE-M Keyboard Extender Maker"
pubDate: 2021-09-09
description: "Tanto si haces streaming, usas shortcuts de teclados en tus programas preferidos o el OBS para hacer tus vídeos en directo,...tal vez te int"
author: "akirasan"
isPinned: false
excerpt: "Tanto si haces streaming, usas shortcuts de teclados en tus programas preferidos o el OBS para hacer tus vídeos en directo,...tal vez te int"
image:
  src: "/content/images/2021/09/KE-M-keyboard-extender-maker-1.jpg"
  alt: "KE-M Keyboard Extender Maker"
tags: ["PCB", "KiCAD", "DIY", "maker"]
---

Tanto si haces streaming, usas shortcuts de teclados en tus programas preferidos o el OBS para hacer tus vídeos en directo,...tal vez te interese este proyecto ;)

Últimamente se ha puesto muy de moda "stremear", y los streamers utilizan este tipo de artefactos que les permite, a golpe de teclado, intercambiar cámaras, pantallas, elementos visuales,...pero también se pueden utilizar para otras cosas, cómo shortcuts en programas de diseño,...

He querido contribuir con un proyecto opensource, que he llamado: **KE-M** **K**eyboard **E**xtender **M**aker. Una PCB con 8 teclas programables con una tecla o una secuencia de teclas.

![](/content/images/2021/09/2021-9-9-22-59-58.gif)

Las principales características de KE-M son:

* 8 teclas configurables en dos posibles modos:
  + *MODEPRESS*: al pulsar se ejecuta una función que envía códigos
  + *MODEHOLDED*: es un doble modo, al pulsar se ejecuta una función, al volver a pulsar se envía otra.
* Pantalla OLED para mostrar mensajes
* LEDs RGBs en la base y en cada una de las teclas
* Librerías de control *U8g2lib.h, AdafruitNeoPixel.h* y *Keyboard.h*

## Componentes hardware

El microprocesador que he utilizado es una pequeña placa de desarrollo [Seeeduino XIAO](https://wiki.seeedstudio.com/Seeeduino-XIAO/) SAMD21 Cortex M0+ que va perfecta para ser integrada en una PCB.

![](/content/images/2021/09/image.png)

El resto de componentes básicos:

* 4x leds RGB WS2812B
* 8x leds RGB WS2812 2020
* 8x teclas transparentes
* 8x interruptores MX para PCB
* 1x pantalla OLED 128x32 (0,91") I2C *(opcional)*

El diseño de la PCB y esquemas en KiCad también lo encontraréis en el repo de Github que esta al final del post.

![](/content/images/2021/09/streamdeck004.jpg)![](/content/images/2021/09/KEM005.jpg)

## El software y configuración

La otra parte del proyecto es el software, así que he desarrollado una clase básica para poder configurar y hacer funcionar **KE-M**. Y vamos a ver la parte y algún ejemplo básico para hacerla funcionar.

![](/content/images/2021/09/image-1.png)

### ¿Qué podemos hacer con la clase KeyboardExtMaker?

Estoy muy oxidado con la programación, así que he hecho lo que he podido con la clase KeyboardExtMaker, que seguramente es mejorable y espero que me ayudéis con diversas contribuciones de mejoras y correcciones en Github ;)

***setKey*** permite definir la configuración del teclado. Por defecto al instanciar la clase se configura todo según PCB, pero está disponible por si se quiere alterar alguna cosa.

***setKeyFunc***, ***setKeyFuncComand1***, ***setKeyFuncComand2*** estos métodos permiten configurar a qué función llamar cuando se pulsa una tecla y que mensaje hay que mostrar en la pantalla OLED.

***setColor\*\*\*\*\**** son métodos para configurar los leds RGBs de la base, de estado de reposo de las teclas y cuando son pulsados.

***loop*** es el método que inicia todo el control del keyboard de forma recurrente

## Algunos ejemplos básicos

### Ejemplo 1

Vamos a definir una función ***void ALT\_TAB()*** que enviará, precisamente la secuencia ALT TAB al pulsar la tecla 4. Por defecto todas las teclas se configuran en *MODE\_PRESS*, lo que quiere decir que solo puede tener un estado y por lo tanto un método al que llamar y un color al ser pulsada:

![](/content/images/2021/09/image-4.png)![](/content/images/2021/09/image-5.png)

### Ejemplo 2

Definimos la tecla 2, en modo *HOLDED*, es decir que cuando se pulsa una vez, enviamos una secuencia de teclas y cuando se vuelve a pulsar, enviamos otra.

Primero ***setKey*** para poner en *MODE\_HOLDED*. Luego definimos el color de la tecla cuando se pulsa una vez (***setColorComand1***) y cuando se pulsa otra vez (***setColorComand2***). Y por último definimos la función a la que llamar cuando se pulse cada uno de los comandos, y el texto que queremos mostrar en la pantalla OLED.

![](/content/images/2021/09/image-2.png)

Aquí hemos definido que la primera llamada ejecute la función ***Send\_KEY2\_func1()*** que hemos creado para enviar una secuencia de teclas gracias a la librería ***Keyboard.h***. Lo mismo para el segundo comando, definimos otra función.

![](/content/images/2021/09/image-3.png)

Demo

El proyecto lo podéis encontrar en este repo en **Github** <https://github.com/akirasan/KEM_KeyboardExtenderMaker>

También está en PCBWay un fabricante de PCB's por si queréis pedirlo ahí: En PCBWay he compartido el proyecto:  
<https://www.pcbway.com/project/shareproject/KE_M_Keyboard_Extender_Maker.html>
