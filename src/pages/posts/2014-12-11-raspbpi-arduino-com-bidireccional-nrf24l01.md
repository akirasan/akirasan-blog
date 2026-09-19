---
layout: ../../layouts/post.astro
title: "Raspberry Pi y Arduino comunicación bidireccional con nrf24l01+"
pubDate: 2014-12-11
description: "Llega el invierno y que mejor manera de preparar un proyecto con RaspberryPi y un Arduino. El objetivo es poder controlar a través de un ent"
author: "akirasan"
isPinned: false
excerpt: "Llega el invierno y que mejor manera de preparar un proyecto con RaspberryPi y un Arduino. El objetivo es poder controlar a través de un ent"
image:
  src: ""
  alt: "Raspberry Pi y Arduino comunicación bidireccional con nrf24l01+"
tags: ["RaspberryPi", "Arduino", "DIY"]
---

Llega el invierno y que mejor manera de preparar un proyecto con RaspberryPi y un Arduino. El objetivo es poder controlar a través de un entorno web el encendido y apagado de la calefacción.

Pero vayamos por pasos, lo primero es conseguir comunicación bidireccional entre la RaspberryPi (servidor domótico) y Arduino (sensor y ejecutor de comandos). Para la comunicación vamos a utilizar módulos **nrf24l01+** y la programación va a ser en Python y Arduino.

![nrf24l01+](/content/images/2014/12/nrf24l01.jpg)

Resumen de pasos a seguir:

* Instalar y activar utilización de GPIO en Raspberry Pi
* Instalar librería Python para utilizar nrf24l01+
* Instalar librería Arduino para utilizar nrf24l01+
* Programación Arduino
* Programación Python en Raspberry Pi
* Pineado y conexión
* Test de comunicación bidireccional

### Instalar y activar utilización de GPIO en Raspberry Pi

Pasos a seguir para instalar las librerías de Python necesarias para utilización de la conexión GPIO (como requisito doy por supuesto que ya tenemos Python instalado)

```auto
sudo apt-get update
sudo apt-get install python-dev
sudo apt-get install python-rpi.gpio
sudo apt-get install python-smbus
sudo apt-get install i2c-tools
```

Añadir al final del fichero ***/etc/modules*** estos dos módulos: **i2c-bcm2708** y **i2c-dev**. Para ello podemos utilizar:

```auto
sudo nano /etc/modules
```

Comentar las lineas con los modulos **spi** y **i2c** en el fichero **/etc/modprobe.d/raspi-blacklist.conf**. Podemos utilizar el comando:

```auto
sudo nano /etc/modprobe.d/raspi-blacklist.conf
```

Identificaremos las entradas siguientes:

```auto
blacklist spi-bmc2708
blacklist i2c-bmc2708
```

Que las anularemos comentándolas de esta forma:

```auto
#blacklist spi-bmc2708
#blacklist i2c-bmc2708
```

### Instalar librería Python para utilizar nrf24l01+

Vamos a instalar la librería Python necesaria para poder comunicarnos y gestionar el módulo nrf24l01+. Existen multitud de variaciones, pero yo he elegido esta, que creo que está bastante actualizada y sigue un mantenimiento:

<https://github.com/jpbarraca/pynrf24>

Lo que haremos será crear un directorio donde vayamos a desarrollar nuestro proyecto y descargar ahí la librería:

```auto
mkdir servidorRasPi
cd servidorRasPi
wget https://raw.githubusercontent.com/jpbarraca/pynrf24/master/nrf24.py
```

Para que esta librería funcione correctamente en nuestra Raspberry, tenemos que hacer un par de pequeñas modificación:

* Hay que sustituir y utilizar la librería **RPi.GPIO** en lugar de la **Adafruit\_BBIO.GPIO** dentro del código del fichero *nrf24.py*. Deberá quedar así:

  ```auto
    # For Raspberry Pi
    import RPi.GPIO as GPIO

    #For BBBB
    #import Adafruit_BBIO.GPIO as GPIO
  ```
* En la instancia por defecto "\_*init\_*" del objeto NRF24, hay que especificar que utilizaremos el modo *GPIO.BCM* mediante la sentencia ***GPIO.setmode(GPIO.BCM)*** que añadimos al final de este código:

  ```auto
    def __init__(self):
        self.ce_pin = "P9_15"
        self.irq_pin = "P9_16"
        self.channel = 76
        self.data_rate = NRF24.BR_1MBPS
        self.data_rate_bits = 1000
        self.p_variant = False  # False for RF24L01 and true for RF24L01P
        self.payload_size = 5  # *< Fixed size of payloads
        self.ack_payload_available = False  # *< Whether there is an ack payload waiting
        self.dynamic_payloads_enabled = False  # *< Whether dynamic payloads are enabled.
        self.ack_payload_length = 5  # *< Dynamic size of pending ack payload.
        self.pipe0_reading_address = None  # *< Last address set on pipe 0 for reading.
        self.spidev = None
        self.using_adafruit_bbio_gpio = GPIO.__name__ == "Adafruit_BBIO.GPIO"
        #>>>> Funcionamiento para RPi
        GPIO.setmode(GPIO.BCM)
  ```

Para que la librería Pyhton de nrf24.py funcione va a requerir que instalemos adicionalmente la librería [py-spidev](https://github.com/doceme/py-spidev), la cual nos permite el acceso al driver SPI del Kernel.

```auto
mkdir python-spi 
cd python-spi 
wget https://raw.github.com/doceme/py-spidev/master/setup.py 
wget https://raw.github.com/doceme/py-spidev/master/spidev_module.c 
sudo python setup.py install
```

Punto importante que me ha llevado bastante horas dar con la solución:  
<http://www.raspberrypi.org/forums/viewtopic.php?f=45&t=17061&start=200>

### Instalar librería Arduino para utilizar nrf24l01+

La instalación de la librería para Arduino es muy sencilla, simplemente hay que descargarse la librería de [aquí](https://github.com/tmrh20/RF24), y descomprimir el contenido en la carpeta plugins de nuestro IDE de Arduino.

### Programación Arduino

Poco que añadir, este es el código para Arduino. Básicamente se trata de enviar un dato variable (en este caso es un contador, pero en el futuro podrá ser un sensor de temperatura) y en cuanto recibamos un comando del servidor (Raspberry Pi) parar el envío y recoger el comando (en este caso solo mostramos el dato recibido que será ON u OFF):

```auto
#include <SPI.h>
#include "nRF24L01.h"
#include "RF24.h"

//Debug
int serial_putc( char c, FILE * ) 
{
  Serial.write( c );
  return c;
} 

//Debug
void printf_begin(void)
{
  fdevopen( &serial_putc, 0 );
}

//nRF24 Cableado utilizado. El pin 9 es CE y 10 a CSN/SS
//     CE       -> 9
//     SS       -> 10
//     MOSI     -> 11
//     MISO     -> 12
//     SCK      -> 13

RF24 radio(9,10);

const uint64_t pipes[6] = {
  0x65646f4e32LL,0x65646f4e31LL};

int a=0;
char b[4];
String str;
int msg[1];
String theMessage = "";
char rev[50]="";

void setup(void) {
  Serial.begin(57600);
  printf_begin();      //Debug

  //nRF24 configuración
  radio.begin();
  radio.setChannel(0x4c);
  radio.setAutoAck(1);
  radio.setRetries(15,15);
  radio.setPayloadSize(32);
  radio.openReadingPipe(1,pipes[0]);
  radio.openWritingPipe(pipes[1]);
  radio.startListening();
  radio.printDetails(); //Debug

  radio.powerUp();
};

void loop() {

  if (radio.available()){
    Serial.println("recibido datos");
    while (radio.available()) {                
      radio.read( &rev, sizeof(rev) );     
      Serial.print(rev);  
    }
    Serial.println();
  }  

  a++;
  str=String(a);
  str.toCharArray(b,4);
  char dato[]="--> DATO ";
  strcat(dato,b);
  radio.stopListening();
  Serial.println("Enviando datos...");
  bool ok = radio.write(&dato,strlen(dato));
  radio.startListening(); 
  delay(2000); 
}
```

### Programación Python en Raspberry Pi

Vamos al lio de la programación en Python. Básicamente se trata de utilizar la librería NRF24, donde a parte de definir el pinout de como tenemos conectado el módulo RF:

```auto
#!/usr/bin/python
# -*- coding: utf-8 -*-
#
#akirasan.net

from nrf24 import NRF24
import time

pipes = [[0x65, 0x64, 0x6f, 0x4e, 0x32], [0x65, 0x64, 0x6f, 0x4e, 0x31]]

radio = NRF24()
radio.begin(0, 0, 25, 18) #Set CE and IRQ pins
radio.setRetries(15,15)
radio.setPayloadSize(32)
radio.setChannel(0x4c)
radio.setPALevel(NRF24.PA_MAX)

radio.openReadingPipe(1, pipes[1])
radio.openWritingPipe(pipes[0])

radio.startListening()
radio.printDetails()
radio.powerUp()
cont=0

while True:
  pipe = [0]

  while not radio.available(pipe):
    time.sleep(0.250)

  recv_buffer = []
  radio.read(recv_buffer)
  out = ''.join(chr(i) for i in recv_buffer)
  print out

  cont=cont+1
  print cont

  if cont==5:
    comando = "ON"
    radio.stopListening()
    print "Envio comando --- ON"
    radio.write(comando)
    radio.startListening()
  if cont==10:
    comando = "OFF"
    radio.stopListening()
    print "Envio comando --- OFF"
    radio.write(comando)
    radio.startListening()
    cont=0
```

### Pineado y conexión

La parte mas importante y que posiblemente ya alguno de vosotros tengáis hecha: la conexión del módulo nrf24l01+ a cada uno de los dispositivos. Para ello, un lugar donde podéis encontrar el pineado que yo he utilizado, es en el artículo de [[ARDUINO + RASPBERRY PI] Switching light with NRF24l01+](http://hack.lenotta.com/arduino-raspberry-pi-switching-light-with-nrf24l01/)de <http://hack.lenotta.com>. Tenéis como conectar a Ardunio y a la RBPi, así como la mejora de soldarle al módulo un condensador para evitar pérdidas en los paquetes que enviamos. Esto ya me lo encontré hace un tiempo y por foros la solución fue esta. Yo he reutilizado una plaquita que tenía por ahí, así que ahí está el condensador:

![arduino_nrf24l01](/content/images/2014/12/Sin-t-tulo.jpg)

Otra imagen de referencia a tener en cuenta es el pineado GPIO en función del modelo de Raspberry Pi que utilicéis. Os dejo una guía:

![1](/content/images/2014/12/Raspberry-Pi-GPIO-pinouts.png)

Y por último, los conectores GPIO que he utilizado en este ejemplo, básicamente he utilizado los que aparecen en post de [[ARDUINO + RASPBERRY PI] Switching light with NRF24l01+](http://hack.lenotta.com/arduino-raspberry-pi-switching-light-with-nrf24l01/).

![raspi_pin_gpio](/content/images/2014/12/Sin-t-tulo-3-.jpg)

### Test de comunicación bidireccional

Aunque no lo he detallado y los códigos de ejemplo ya están preparados para hacer comunicación en un sentido y otro, lo ideal es comenzar por realizar diferentes test de *Arduino-->RBPi* y *RBPi-->Arduino* de forma unitaria para comprobar que todo está OK. Esto es lo que primero estube realizando.

![3](/content/images/2014/12/IMG_20141129_231146_1920x1080.jpg)

Si váis directamente a probar el código de este ejemplo sin realizar previamente las pruebas anteriores,...pues nada, vamos allá:

Lo primero que podemos hacer es ejecutar el código de Arduino y activar el monitor serial para los mensajes que intercambia y los puntos de información de debug que hemos puesto.

Por otro lado desde un terminal ejecutamos el código Python que hemos generado. Os aconsejo ejecutarlo como *sudo* ya que el usuario por defecto de "pi" no tiene permisos (es un punto a tener en cuenta mas adelante):

```auto
sudo python srv_raspi.py
```

Y si todo ha ido bien, tras ver los datos de configuración del módulo *nrf24l01+* tenemos que observar, tanto en la consola de Arduino como en la consola de la Raspberry Pi, algo similar a esto:

![comunicacion_rbpi_arduino](/content/images/2014/12/rpi2arduino.JPG)

Donde vemos como la Raspberry Pi va recibiendo un mensaje `"--> DATO nnn"` y cuando llega a contar 5 paquetes recibidos envía un comando `"ON"` al Arduino, el cual deja reflejado en la consola. Luego la RBPi sigue recibiendo datos y al paquete número 10 envía otro comando `"OFF"` que también vemos en la consola de Arduino. Y así sucesivamente.

### Conclusión

Este ejemplo es muy sencillo, pero lleva tiempo implementarlo, ya que la información por internet no es muy clara y en las pruebas unitarias de comunicación me he encontrado con algunos problemas, por eso os pongo un último apartado con *Notas adicionales*

La idea de este ejemplo, como he comentado al inicio, es controlar el encendido y apagado de la calefacción de forma remota, a la vez que disponemos de un sensor de temperatura que registra en información. Así que, si no me aburro y lo abandono, este proyecto irá teniendo otras entregas.

### Notas adicionales

Hay que revisar muy bien el cableado y ver que según la versión de RBPi que se disponga el pin CE debe estar en el GPIO 25:

```auto
radio.begin(0, 0, 25, 18) #Set CE and IRQ pins
```

Esto es especialmente importante a la hora de enviar datos desde RBPi al Arduino. Parece ser que no importa tanto cuando es la RBPi quien recibe datos, porque siempre me funcionó a la primera en las pruebas unitarias. Va relacionado si utilizamos GPIO.setmode(GPI.BOARD) o GPIO.setmode(GPI.BCM), sentencia que añadimos a la librería *nrf24.py*.

El tema del condensador en la parte de Arduino, va en función del tipo de Arduino que utilicéis. En mis pruebas anteriores a otro proyecto inacabado, el resultado fue que si quería utilizar una Ardunio Mini Pro, era condición necesaria soldarle un condensador. En aquel caso también muy necesario si se alimenta mediante batería.

Ojo al utilizar Arduino Mini, los cuales tienen problemas con la interface SPI y posiblemente os darán problemas a la hora de conectar el módulo nrf24.

## UPDATE [12/04/2015]

Un error del estilo:

```auto
File "/home/pi/nrf24.py", line 391, in begin
self.setRetries(int('0101', 2), 15)
File "/home/pi/nrf24.py", line 391, in setRetries
self.write_register(NRF24.SETUP_RETR, (delay & 0xf) << NRF24.ARD | (count & 0xf)
File "/home/pi/nrf24.py", line 246, in write_register
return self.spidev.xfer2(buf)[0]
IOError: [Errno 22] Invalid argument
```

![](/content/images/2015/04/nrf24_error_spidev.JPG)

El módulo de Python spidev está *"roto"* desde la versión del Kernel 3.15.x. He solucionado el tema instalando el módulo spidev con estos pasos:

```auto
mkdir py-spidev
cd py-spidev
wget https://github.com/doceme/py-spidev/archive/master.zip
unzip master.zip
cd py-spidev-master
sudo python ./setup.py install
```
