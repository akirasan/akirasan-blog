---
layout: ../../layouts/post.astro
title: "Sensor láser de distancia VL53L0X"
pubDate: 2018-01-21
description: "Vamos a probar el sensor de distancia VL53L0X, un pequeño módulo integrado que permite medir la distancia de una forma precisa y sin afectar"
author: "akirasan"
isPinned: false
excerpt: "Vamos a probar el sensor de distancia VL53L0X, un pequeño módulo integrado que permite medir la distancia de una forma precisa y sin afectar"
image:
  src: "/content/images/2018/01/upload-1516549624.jpg"
  alt: "Sensor láser de distancia VL53L0X"
tags: []
---

Vamos a probar el sensor de distancia VL53L0X, un pequeño módulo integrado que permite medir la distancia de una forma precisa y sin afectarle la reflactancia del objeto. Se puede medir distancias absolutas  
hasta 2 metros.

![](/content/images/2018/01/upload-1516549600.jpg)

Aquí podéis consultar el [datasheet](http://www.st.com/content/st_com/en/products/imaging-and-photonics-solutions/proximity-sensors/vl53l0x.html) del integrado.

#### Conexiones

Si vemos el pinout de este módulo veremos que funciona mediante bus [I2C](https://es.wikipedia.org/wiki/I%C2%B2C), por lo que únicamente nos harán falta dos cables para la comunicación y otros dos para la alimentación.

En mi prueba he utilizado un Arduino UNO, por lo que tenemos que asegurarnos de cuales son los pines I2C con los que trabaja Arduino que estamos utilizando. En este caso **SDA - pin A4** y **SCL - pin A5**.

El voltaje de trabajo puede estar entre 2.6V y 5.5V, así que podemos alimentarlo directamente desde nuestro Arduino.

#### Librerías disponibles

Si hacemos una búsqueda de VL53L0X en el IDE de Arduino, encontramos varias propuestas. Vamos a hacer un repaso de lo que nos sale:

![gestor_librerias_arduino](/content/images/2018/01/gestor_librerias_arduino.jpg)

**Adafruit\_VL53L0X**  
<https://github.com/adafruit/Adafruit_VL53L0X>

**STM32duino VL53L0X**  
<https://github.com/stm32duino/VL53L0X>  
Ésta librería no es compatible con Arduino, únicamente sirve para programar el STMicroelectronics STM32: <https://en.wikipedia.org/wiki/STM32>  
Depende de otra librería que tendrémos que instalar y que podemos encontrar también el en IDE:

* Proximity\_Gesture: <https://github.com/stm32duino/Proximity_Gesture>  
  ![gestor_librerias_arduino_2](/content/images/2018/01/gestor_librerias_arduino_2.jpg)

**STM32duino X-NUCLEO-53L0A1**  
<https://github.com/stm32duino/X-NUCLEO-53L0A1>  
Ésta librería depende de las dos anteriores, por lo tanto está pensada para microcontroladores STM32:

* VL53L0X: <https://github.com/stm32duino/VL53L0X>
* Proximity\_Gesture: <https://github.com/stm32duino/Proximity_Gesture>

**VL53L0X by Pololu**  
<https://github.com/pololu/vl53l0x-arduino>

Llegados a este punto, solamente nos queda probar la de Adafruit y la de Pololu.

#### Adafruit\_VL53L0X

Esta librería únicamente nos trae un ejemplo muy sencillo:

![ejemplo_adafruit_vl53l0x](/content/images/2018/01/ejemplo_adafruit_vl53l0x.jpg)

![vl53l0x_adafruit_code](/content/images/2018/01/vl53l0x_adafruit_code.jpg)

El código nos motrará la distancia que está midiendo el sensor y nos lo mostrará por el Serial. Simple y fácil.

A partir de aquí toca investigar un poquito mas sobre este dispostivo, ya que por lo visto tiene varias amplicaciones prácticas (a parte de medir distancia), control de gestos, detección de objetos, mediciones para sistemas de enfoque, etc.
