---
layout: ../../layouts/post.astro
title: "Sensor de movimientos HC-SR501 conectado por GPIO"
pubDate: 2016-07-20
description: "Vamos a conectar un sensor **HC-SR501** cómo el de la foto a una , un clon chino de la RaspberryPi, que tiene también una serie de puertos G"
author: "akirasan"
isPinned: false
excerpt: "Vamos a conectar un sensor **HC-SR501** cómo el de la foto a una , un clon chino de la RaspberryPi, que tiene también una serie de puertos G"
image:
  src: "/content/images/2016/07/IMG_20160720_175953_20160720_192232.jpg"
  alt: "Sensor de movimientos HC-SR501 conectado por GPIO"
tags: ["orangepi", "seguridad", "python", "RaspberryPi"]
---

![](/content/images/2016/07/PhotoGrid_1469036264382.jpg)

Vamos a conectar un sensor **HC-SR501** cómo el de la foto a una [OrangePi](http://www.orangepi.org/), un clon chino de la RaspberryPi, que tiene también una serie de puertos GPIO's. La librería que vamos a utilizar funciona para esta placa, pero la idea y concepto es aplicable también a una Raspberry Pi utilizando por ejemplo la librería Python [RPi.GPIO](https://pypi.python.org/pypi/RPi.GPIO)

Lo primero que hacemos es conocer el pinout del HC-SR501, que prácticamente tiene dos pines para la alimentación (GND y VCC) y un pin de salida (OUT) que será el que nos indique si ha detectado algún movimiento.

Luego lleva un par de resistencia variables o potenciómetros que nos permiten ajustar los parámetros de sensibilidad y duración de la señal, es decir, durante cuanto tiempo vamos a querer que en pin OUT tener la salida informada. Esto irá configurado dependiendo de la utilización y disposición del HC-SR501.

![](/content/images/2016/07/hc-sr501-pinout.png)

Primero y si no lo tenemos instalado, es instalar `git`:

```auto
orangepi@OrangePI:~$ sudo apt-get install git
```

Vamos a utilizar la librería en Python **orangepi\_PC\_gpio\_pyH3** para acceder los puerto de conexión GPIO's. Ésta librería la podeis encontrar en el GitHub del usuario [duxingkei33](https://github.com/duxingkei33/orangepi_PC_gpio_pyH3), y básicamente es una adaptación de la librería [pyA20 0.2.1](https://pypi.python.org/pypi/pyA20) que se utiliza en la placa [A20-OLinuXino-MICRO](https://www.olimex.com/wiki/A20-OLinuXino-MICRO).

Vamos a clonar primero el repositorio:

```auto
git clone https://github.com/duxingkei33/orangepi_PC_gpio_pyH3.git
```

Y ahora para instalar ejecutamos:

```auto
cd orangepi_PC_gpio_pyH3
sudo python setup.py install
```

Ahora que ya tenemos el software vamos a ver como conectamos el hardware. El sensor HC-SR501 funciona a 5v, por lo buscamos los pines que nos proporcionan la alimentación (en la imagen de abajo corresponden a los pines 4 y 6 de la referencia *CONN.* de color azul). El pin con el estado del sensor lo he conectado al puerto 7 de GPIO (en la imagen de abajo corresponde dentro de la línea azul *CONN.* al conector número 7)

![](/content/images/2016/07/orange-pi-pc-pinout.png)

*(La imagen es de <http://electroniqueamateur.blogspot.com.es>)*

![](/content/images/2016/07/IMG_20160720_180051_20160720_192239.jpg)  
![](/content/images/2016/07/IMG_20160720_180122_20160720_192237.jpg)

Una vez tenemos conectado nuestro sensor HC-SR501 a nuestra placa OrangePi, vamos a generar un pequeño script de ejemplo para leer el valor del conector GPIO 7, o tal y como lo veremos en el código el `port.PA6`:

```auto
from pyA20.gpio import gpio
from pyA20.gpio import port
from pyA20.gpio import connector

gpio.init() #Inicializamos el modulo. Siempre lo primero

gpio.setcfg(port.PA6, gpio.INPUT) #Configura PA6 como entrada

while True:
  if gpio.input(port.PA6) == 1:
    print "PA6/PIN 7 = 1"
  else:
    print "PA6/PIN 7 = 0"
```

Si vemos en la imagen anterior con el pinout, el conector GPIO 7 corresponde al nombre del puerto PA6 (línea *PORT* de color amarillo de la imagen). La librería pyA20 utiliza esta nomenclatura de puertos ya que la placa a la que está orientada originariamente (no ésta modificación/adaptación) tiene diferentes fuentes de GPIO's y es una forma de darles un nombre único dentro de todas las conexiones.

El ejemplo es muy sencillo y únicamente lo que hace es leer el valor del sensor, que puede ser *"0"* (no detecta movimiento) o *"1"* (se detecta movimiento). **Problema que veo por ahora: la inicialización del módulo requiere root**, por lo que el script de *Python* lo tenemos que ejecutar con `sudo`

Recordad que el sendor HC-SR501 tenía dos potenciómetros los cuales nos ayudan a calibrar la sensibilidad y el tiempo en el que dejamos en HIGH la salida en caso de ser activado.

Cómo información adicional, si miramos dentro del código de la librería **orangepi\_PC\_gpio\_pyH3** el fichero [mapping.h](https://github.com/duxingkei33/orangepi_PC_gpio_pyH3/blob/master/pyA20/gpio/mapping.h) tiene la relación entre PUERTO y CONECTOR. Por ejemplo, el que yo he utilizado lo encontraremos definido así (*"PA6" cómo 7*):

```auto
...
{   "PA6",  SUNXI_GPA(6),  7   },
...
```
