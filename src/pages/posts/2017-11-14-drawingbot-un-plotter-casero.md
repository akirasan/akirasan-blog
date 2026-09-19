---
layout: ../../layouts/post.astro
title: "DrawingBot - Un plotter casero OpenSource"
pubDate: 2017-11-14
description: "***Post en construcción*** *---Iré actualizando, por lo que falte información específica, pero seguro que te sirve para desarrollar el proye"
author: "akirasan"
isPinned: false
excerpt: "***Post en construcción*** *---Iré actualizando, por lo que falte información específica, pero seguro que te sirve para desarrollar el proye"
image:
  src: "/content/images/2017/11/IMG_20170701_114913.jpg"
  alt: "DrawingBot - Un plotter casero OpenSource"
tags: ["Arduino", "3dprinting", "maker", "tools", "DIY"]
---

***Post en construcción*** *---Iré actualizando, por lo que falte información específica, pero seguro que te sirve para desarrollar el proyecto---*

---

**DrawingBot** es una versión Open Hardware de la versión comercial AxiDraw. No es mas que un sencillo y moderno plotter, capaz de escribir o dibujar sobre casi cualquier superficie plana mediante un bolígrafo, rotulador, lápiz, etc,...

También existe otro proyecto llamado 4xiDraw que está bastante mejor documentado y que tal vez encuentras mas información en [Instructables](http://www.instructables.com/id/4xiDraw/).

#### Ingredientes para el montaje

Para realizar el proyecto vas a necesitar:

**Estructura**

* Juego de piezas impresas
* 2x Varillas M8 x 450mm (Eje X)
* 2x Varillas M8 x 350mm (Eje Y)
* 2x Varillas 3.5mm (Eje Z) (Se pueden encontrar en las antiguas CDROM o utilizar dos clavos de ese diámetro como he usado yo)
* 1x Varilla roscada M8 x 470mm (para dar consistencia a la estructura)
* 8x Rodamientos lineales Lm8uu
* 2x Engranajes GT2 16 (para los motores NEMA17)
* 5x Rodamientos 624zz
* 2000mm (aprox) Correa GT2
* Tornillos, tuercas y arandelas para M3, M4 **(*pendiente inventariar*)**
* 4x Tuercas M8
* 4x Arandelas para M8

**Los diseños para imprimir** de DrawingBot los podéis bajar de [Thingiverse](https://www.thingiverse.com/thing:1517211)

**Electrónica**

* 2x Motores NEMA 17
* 1x Servo sg90
* 2x EndStop *(**opcional**, yo no los he utilizado ya que no hago homing al inicio de la impresión)*
* 1x Kit Arduino UNO + Shield CNC + drivers motores DRV4988 (he optado por esta solución mucho mas simple y sencilla)
* Fuente alimentación 12V a 2A

#### Hardware

Vamos a revisar específicamente los aspectos importantes a la hora montar la electrónica.

Vamos a utilizar el kit Arduino UNO + Shield CNC y los correspondientes drivers 4988. Este kit lo podéis encontrar muy económico por ejemplo en [aliexpress](https://es.aliexpress.com/item/Free-shipping-cnc-shield-v3-engraving-machine-3D-Printer-4pcs-A4988-driver-expansion-board-for-Arduino/32222497855.html)

Montaremos la shield sobre el Arduino y colocaremos tres jumpers de configuración a cada driver (son los que están justo debajo de cada driver y configuran el número de pasos a los que va a trabajar cada motor).

![](/content/images/2017/07/IMG_20170703_192449-1.jpg)  
![](/content/images/2017/07/IMG_20170703_192525-1.jpg)

Fijaros solo en un detalle, por si luego tenéis algún problema. Si tenemos así orientada la electrónica respecto a la estructura del plotter, el motor de la izquierda de la imagen está conectado como eje X, y el de la derecha como eje Y.

![](/content/images/2017/07/IMG_20170703_192722-1.jpg)

El servo sg90, lo vamos a tener que conectar a los pines de alimentación (5v) y GND y el cable de PWM (normalmente de color amarillo anaranjado) lo colocaremos en el pin de endstops de Z+. Ésto es así, porque el firmware grbl que vamos a utilizar está modificado para ese pin (ojo, que he visto varios comentarios, que en función del Arduino, éste pin puede variar, para Arduino UNO no hay problema ni hacer modificaciones)

![](/content/images/2017/07/IMG_20170703_192638-1.jpg)

#### Software de control

El firmware que tenemos que subir a Arduino es una versión del Grbl modificada para poder trabajar con el servo, que subierá y bajará el lápiz cuando toque [grbl 0.9i with Servo motor support](https://github.com/robottini/grbl-servo). A parte tendremos que hacer un pequeño cambio en el fichero `config.c` para que esté configurado como una máquina CoreXY:

Buscamos la linea donde aparece la definición desactivada y la activamos, eliminado el "//" (código comentado), quedando de esta forma:

`// Enable CoreXY kinematics. Use ONLY with CoreXY machines.  
// IMPORTANT: If homing is enabled, you must reconfigure the homing cycle #defines above to  
// #define HOMING_CYCLE_0 (1<<X_AXIS) and #define HOMING_CYCLE_1 (1<<Y_AXIS)  
// NOTE: This configuration option alters the motion of the X and Y axes to principle of operation  
// defined at (http://corexy.com/theory.html). Motors are assumed to positioned and wired exactly as  
// described, if not, motions may move in strange directions. Grbl assumes the CoreXY A and B motors  
// have the same steps per mm internally.

# define COREXY // Default disabled. Uncomment to enable.`

### ¿Cómo paso un dibujo al DrawingBot?

Yo he utilizado Inkscape, que además podemos encontrar plugins (o extensiones, como se llama), para generar el fichero GCode que enviaríamos a nuestro DrawingBot.

Tenemos que pensar que nuestra imagen **siempre tiene que ser vectorial**. Es la única forma de que puede ser interpretados los paths y generados los comandos GCode.

Los paths son tratados cómo contornos, es decir, que si escribimos unas letras y las vemos rellenas en un color, el relleno no es una *característica vectorial* interpretable.

**¿Cómo relleno los objetos con trazos vectorizados?**

Para rellenar objetos, utilizo **la extensión *Hatch Fill*** que hay desarrollada para el **EggBot** y que podéis descargar en su repositorio GitHub: <https://github.com/evil-mad/EggBot/releases/>.

Es una forma sencilla de rellenar figuras, hay otros métodos, pero éste me ha resultado fácil. Esto es importante porque tened en cuenta que el plotter va a dibujar contornos, no el relleno. Mas adelante veréis un ejemplo con las letras rellenas.

**¿Cómo paso de un dibujo vectorial al GCODE?**

Para pasar de imagen vectorizada a GCode, utilizo la extensión [J Tech Laser Tool](https://jtechphotonics.com/?page_id=1980), que generá el gCode a partir de la imagen vectorizada. Remarco los parámetros importantes:

* **M03**: Para que libere el lápiz
* **M05**: Para que lo levante y poder moverse libremente

![](/content/images/2017/07/Selecci-n_004.png)  
![](/content/images/2017/07/Selecci-n_003.png)

Ya tenemos nuestro fichero listo para ser procesado por nuestro DrawingBot.

**¿Cómo envío el fichero GCODE a procesar por el DrawingBot?**

Una de las aplicaciones mas sencillas para enviar el GCODE a nuestro firmware GRBL es [Universal GcodeSender](https://github.com/winder/Universal-G-Code-Sender) basado en Java y compatible con GRBL.

La aplicación es muy sencilla, una vez descargada la ejecutamos y revisamos el puerto donde tenemos conectado nuestro DrawingBot y nos conectamos. Veremos en ese momento que si todo va bien, nos aparecerán las opciones de configuración de *grbl*. También si enviamos el comando "$$" podremos verlas. Estas opciones están guardadas en la memoria interna del Arduino. Antes de enviar nuestra primera prueba, vamos a tener que verificar y posiblemente cambiar algunos de los valores de los parámetros.

Para cambiar el valor de un parámetro es muy sencillo: **< $numero\_parametro > = < nuevo\_\_valor >**. Veamos, los parámetros que hay que revisar son:
