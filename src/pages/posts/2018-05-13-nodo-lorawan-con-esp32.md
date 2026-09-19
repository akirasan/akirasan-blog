---
layout: ../../layouts/post.astro
title: "Nodo LoRaWAN con ESP32"
pubDate: 2018-05-13
description: "Si, es cierto, montar un nodo LoRaWAN con un ESP32 que lleva WiFi y BLE es un poco bruto, desde el punto de vista de concepto ;) pero por ah"
author: "akirasan"
isPinned: false
excerpt: "Si, es cierto, montar un nodo LoRaWAN con un ESP32 que lleva WiFi y BLE es un poco bruto, desde el punto de vista de concepto ;) pero por ah"
image:
  src: "/content/images/2018/05/IMG_20180513_102921-1.jpg"
  alt: "Nodo LoRaWAN con ESP32"
tags: ["LoRa", "LoRaWan", "IoT", "ESP32"]
---

Si, es cierto, montar un nodo LoRaWAN con un ESP32 que lleva WiFi y BLE es un poco bruto, desde el punto de vista de concepto ;) pero por ahora es lo que tengo, jejeje.

Previamente ya hemos [preparado el IDE de Arduino para usar los módulos ESP32 con LoRa](http://akirasan.net/preparando-arduino-ide-para-esp32-lora/). Luego hemos montado nuestro [minigateway LoRaWAN basado también en un módulo ESP32 con LoRa](http://akirasan.net/minigateway-lora-monocanal-con-esp32/). Ahora cómo último paso, nos queda preparar un nodo. Un nodo que comience a enviar información a través de LoRa.

Pero primero, un poco de teoría importante de cómo se pueden activar los nodos en *The Things Network*. Ha tocado leer información y revisar algunos post cómo <https://www.thethingsnetwork.org/forum/t/big-esp32-sx127x-topic-part-2/11973> para entender como funciona LoRaWAN.

Para montar el nodo voy a utilizar la **librería LMIC-Arduino** <https://github.com/matthijskooijman/arduino-lmic> y te das cuenta que hay mas conceptos e información a entender, como por ejemplo: los modos de activación del nodo, que puede ser mediante **ABP** o **OTAA**:

**ABP: Activation By Personalisation**  
En ABP un dispositivo no necesita los identificadores de *DevEUI*, *AppEUI* y *AppKey*. En cambio las claves de sesión, *NwkSKey* y *AppSKey* si, y las tiene preprogramadas el dispositivo, el cual ya estaría preregistrado en la red. Cuando el dispositivo se quiere comunicar, lo hace usando esas claves de sesión sin tener que usar ningún procedimiento de unión a la red.

**OTAA: Over-The-Air Activation**  
El modo de activación OTAA, el dispositivo recibe los identificadores *DevEUI*, *AppEUI* y *AppKey*. De esta forma genera las claves de sesión *NwkSKey* y *AppSKey* (ojo de no confundir *AppKey* con *AppSKey*, solo les separa una "s", pero son diferentes). Para activarse, el dispositivo envía una solicitud de unión y usa la respuesta para generar las claves de sesión *NwkSKey* y *AppSKey*. El dispositivo puede almacenar esas claves y utilizándolas para comunicarse. Si se pierden o la red elige caducarlas, el dispositivo puede volver a unirse y generar nuevas claves de sesión.

Para aclarar algunos conceptos que han salido en la explicación de antes:

* **EUI: Extended Unique Identifier**: ID único global
* **DevEUI: Device EUI**: Fijado por el fabricante, único por dispositivo
* **AppEUI: Application EUI**: Identifica la aplicación de destino
* **AppKey: Application Key**: Usado en el modo OTAA para generar las claves de sesión
* **DevAddr: Device Address**: Identifica a un dispositivo dentro de una determinada red
* **NwkSKey: Network Session Key**: Encripta los paquetes de metadatos
* **AppSKey: Application Session Key**: Encripta los paquetes de *payload*

Vamos al lio!!!

### Instalación del software

Cómo he comentado vamos a utilizar la **librería LMIC-Arduino** <https://github.com/matthijskooijman/arduino-lmic>. Cómo es una librería, descargamos el ZIP y lo instalamos utilizando el gestor de librerías de Arduino.

![Selecci-n_186](/content/images/2018/05/Selecci-n_186.png)

![Selecci-n_187](/content/images/2018/05/Selecci-n_187.png)

Con ésto ya tenemos ejemplos cargados en nuestro IDE de Arduino. Vamos a comenzar por el método **ABP: Activation By Personalisation**

![Selecci-n_188](/content/images/2018/05/Selecci-n_188.png)

¿Qué vamos a necesitar configurar en el código?. Principalmente las claves de sesión *NwkSKey* y *AppSKey*, y el *DevAddr* que identificará a nuestro nodo en TTN.

![Selecci-n_189](/content/images/2018/05/Selecci-n_189.png)

¿Y de dónde sacamos esas claves?. Para conseguir esas claves debemos acceder a nuestra cuenta de The Things Network, crear una aplicación y un dispositivo.

![Selecci-n_190](/content/images/2018/05/Selecci-n_190.png)

![Selecci-n_191](/content/images/2018/05/Selecci-n_191.png)

Ahora ya tenemos creada nuestra primera aplicación. El siguiente paso será añadir nuestro nodo/dispositivo.

![Selecci-n_192](/content/images/2018/05/Selecci-n_192.png)

![Selecci-n_193](/content/images/2018/05/Selecci-n_193.png)

Cuando registramos un nuevo dispositivo, se generarán automáticamente las claves únicas. Pero si os fijáis el método de activación por defecto es *OTAA*, así que una vez creada modificaremos por *Settings* el método a *ABP*

![Selecci-n_194](/content/images/2018/05/Selecci-n_194.png)

![Selecci-n_195](/content/images/2018/05/Selecci-n_195.png)

![Selecci-n_196](/content/images/2018/05/Selecci-n_196.png)

Ahora ya está!!! ya disponemos de las claves que necesitamos trasladar al código de nuestro dispositivo

![Selecci-n_197](/content/images/2018/05/Selecci-n_197.png)

Hay que tener en cuenta la conversión de esos códigos en hexadecimal. Por ejemplo el la clave:

```auto
7E EC 2C 35 2F D6 A2 C4 B7 A0 CB 7F D5 3B BB 08
```

Se tiene que pasar de la siguiente forma en código Arduino:

```auto
 0x7E, 0xEC, 0x2C, 0x35, 0x2F, 0xD6, 0xA2, 0xC4, 0xB7, 0xA0, 0xCB, 0x7F, 0xD5, 0x3B, 0xBB, 0x08
```

![Selecci-n_199](/content/images/2018/05/Selecci-n_199.png)

Con el código ya listo, solo falta subirlo a nuestro dispositivo y veremos como empieza a enviar información a nuestra aplicación en The Things Network, usando nuestro minigateway,...IMPRESIONANTE!!!

...pues no. Llegado a este punto, cuando subo el código, por el *Serial* obtengo errores

```auto
Guru Meditation Error: Core  1 panic'ed (LoadProhibited)
```

![Selecci-n_200](/content/images/2018/05/Selecci-n_200.png)

Revisando el código, veo que ***Pin mapping*** no está bien definido para la TTGO

![Selecci-n_202](/content/images/2018/05/Selecci-n_202.png)

Así que utilizando la información de [Big ESP32 + SX127x topic part 2](https://www.thethingsnetwork.org/forum/t/big-esp32-sx127x-topic-part-2/11973) sacamos información de los pines y modificamos el código

![Selecci-n_203](/content/images/2018/05/Selecci-n_203.png)

```auto
// Pin mapping
const lmic_pinmap lmic_pins = {
    .nss = 18,
    .rxtx = LMIC_UNUSED_PIN,
    .rst = 14,
    .dio = {26, 33, 32},
};
```

Una vez corregido esto, subimos nuevo firmware y...otro error!!!

![Selecci-n_204](/content/images/2018/05/Selecci-n_204.png)

```auto
Starting
FAILURE 
/home/akirasan/Arduino/libraries/arduino-lmic-master/src/lmic/radio.c:689
```

Uff!!!,...y ahora que??. Pues después de buscar mucho, he encontrado una aportación del usuario *vshymanskyy* en <https://www.thethingsnetwork.org/forum/t/big-esp32-sx127x-topic-part-1/10247/130>

![Selecci-n_205](/content/images/2018/05/Selecci-n_205.png)

Consiste en abrir la librería LMIC y cambiar el código fuente, forzando a **inicializar el SPI indicando los pines del módulo LoRa**.

![Selecci-n_206](/content/images/2018/05/Selecci-n_206.png)

En mi modificación lo he dejado documentado, cada parámetro del **SPI.begin(pin\_SCK, pin\_MISO, pin\_MOSI, pin\_SS)**. Esta información puede variar dependiendo de la placa que utilicéis.

```auto
//SPI.begin();
//---> begin(SCK,MISO,MOSI,SS)
SPI.begin(5,19,27,18);
```

Espero que se solucione rápido y no tengamos que estar tocando código.

Bueno llegó la hora de verificar cómo nuestro nodo está enviado información a través del gateway de LoRa. Para ello accedemos a la consola y veremos con están llegando los paquetes.

El mensaje que enviamos viaja en el campo *PAYLOAD*, y como se puede ver es un hexadecimal que codifica el mensaje *ASCII*

![Selecci-n_207](/content/images/2018/05/Selecci-n_207.png)

Para *decodificar* esa información y que sea entendible a simple vista, podemos desarrollar una función a nivel de aplicación que decodifique.

![Selecci-n_208](/content/images/2018/05/Selecci-n_208.png)

Cambiaremos esa función por esta otra que he conseguido [en esta entrada de ictoblog.nl](https://ictoblog.nl/2018/01/13/my-chinese-esp32-sx1276-board-dht11-connected-to-the-things-network-1).

```auto
function Decoder(bytes, port) {
  var text = String.fromCharCode.apply(null, bytes);
  return {
    message: text
  };
}
```

![Selecci-n_209](/content/images/2018/05/Selecci-n_209.png)

Esta forma cuando consultamos la información *Data* a nivel de aplicación (no a nivel de *Device* que ahí seguirá viéndose en hexadecimal), podremos tener la decodificación del mensaje.

![Selecci-n_211](/content/images/2018/05/Selecci-n_211.png)

A hasta aquí es todo!!!. Ya tenemos un nodo funcionando.

Creo que las librerías de TheThingsNetworks para trabajar con Arduino son mejores que ésta para ESP32. Creo que lo siguiente será utilizar un nodo LoRa y conectarlo a un Arduino, montarle un sensor sencillo (de temperatura, humedad, luz,...) para recoger datos.
