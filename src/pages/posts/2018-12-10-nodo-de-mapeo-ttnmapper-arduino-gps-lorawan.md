---
layout: ../../layouts/post.astro
title: "Nodo de mapeo TTNMapper: Arduino + GPS + LoRaWAN"
pubDate: 2018-12-10
description: "Si has utilizado la aplicación para el móvil de  para mapear la cobertura de nodos LoRaWAN , tal vez te gustaría dar un paso mas y construir"
author: "akirasan"
isPinned: false
excerpt: "Si has utilizado la aplicación para el móvil de  para mapear la cobertura de nodos LoRaWAN , tal vez te gustaría dar un paso mas y construir"
image:
  src: "/content/images/2018/12/IMG_20181021_134030-1.jpg"
  alt: "Nodo de mapeo TTNMapper: Arduino + GPS + LoRaWAN"
tags: ["IoT", "LoRa", "LoRaWan", "TTN"]
---

Si has utilizado la aplicación para el móvil de [TTNMapper](https://ttnmapper.org/) para mapear la cobertura de nodos LoRaWAN [The Things Networks](https://www.thethingsnetwork.org/), tal vez te gustaría dar un paso mas y construir un nodo TTN capaz de enviar la información a TTNMapper sin necesidad de usar la aplicación del móvil, sino un tracker autónomo.

Para crear nuestro *nodotrackerGPS* LoRaWAN vamos a utilizar tres piezas básicas: un módulo LoRa RFM95W, un Arduino mini Pro y un módulo GPS. Con estos ingredientes bien conectados, un pequeño programa y una configuración en el cloud de [The Things Networks](https://www.thethingsnetwork.org/), lo tendremos todo listo.

### Configuración en The Things Network

Cómo el programa que utilizaremos en Arduino necesita parte de la definición de TTN, vamos a realizar primero una configuración básica en TTN: una aplicación y un devices.

La configuración de una aplicación para nuestro proyecto de mapeo en TTNMapper va ser prácticamente igual a cualquier otra aplicación.

![](/content/images/2018/12/add_app_ttn_ttnmapper1.JPG)

Creamos ahora un nodo *"TTNMapper"* (***Devices***):

![](/content/images/2018/12/add_app_ttn_ttnmapper2.JPG)![](/content/images/2018/12/add_app_ttn_ttnmapper3-1.JPG)![](/content/images/2018/12/add_app_ttn_ttnmapper4.JPG)

Ahora configuraremos el script ***Decoder()*** que nos permitirá procesar y decodificar el ***payload*** (datos que nos envía el nodo) para ser enviado posteriorment a TTNMapper en formato JSON.

![](/content/images/2018/12/add_app_ttn_ttnmapper5.JPG)

El código JavaScript que he implementado lo tenéis aquí:

```auto
function Decoder(bytes, port) {
  // Decode an uplink message from a buffer
  // (array) of bytes to an object of fields.
var decoded = {};

lat_decode = ((bytes[0]) << 24)
+ ((bytes[1]) << 16)
+ ((bytes[2]) << 8)
+ ((bytes[3]));

lon_decode = ((bytes[4]) << 24)
+ ((bytes[5]) << 16)
+ ((bytes[6]) << 8)
+ ((bytes[7]));

decoded.latitude = lat_decode / 1000000;
decoded.longitude = lon_decode / 1000000;

decoded.altitude = ((bytes[8]) << 8) + ((bytes[9]));
decoded.hdop = bytes[10] / 10;
decoded.sats = bytes[11];

return decoded;
}
```

Este código puede variar un poco en función de los datos que enviéis, los cuales tienen su correlación y origen en el código del nodo tracker. Para mas info sobre este tema podéis revisar anteriores artículos **[Cómo transmitir datos en LoRaWAN, reduciendo el payload](http://akirasan.net/la-importancia-de-un-buen-payload-en-lorawan/)** y **[Enviar coordenadas GPS por LoRaWAN TTN](http://akirasan.net/enviar-coordenadas-gps-por-lorawan-the-things-network/)**.

El siguiente paso que debemos configurar en nuestra aplicación **es la integración con TTNMapper** (***Integrations***). En TTN encontraremos conectores a diferentes plataformas donde podremos enviar los datos que recibimos, como una base de datos propia de The Things Industries (Data Storage), IFTTT, OpenSensors.io, integración con HTTP,…y evidentemente la integración con TTN Mapper:

![](/content/images/2018/12/add_app_ttn_ttnmapper6-1.JPG)![](/content/images/2018/12/add_app_ttn_ttnmapper7-1.JPG)

La definición de la integración es bastante sencilla: un ID y un e-mail. Podemos indicar también si utilizamos un puerto específico por el que recogemos los datos que nos envía el nodo y también algo interesante cuando estamos haciendo pruebas es definir un nombre de experimento (***Experiment name***).

![](/content/images/2018/12/add_app_ttn_ttnmapper8-1.JPG)

Si definimos un nombre de experimento, los datos son enviados a TTNMapper, pero no se integran en el mapa general. Esta opción nos permite validar los datos que estamos enviando, para ver estos experimentos, se puede acceder al listado que encontraremos en la web de TTNMapper.

Una vez definida nuestra integración, y si todo está OK, veremos que su estado es *Running*.

![](/content/images/2018/12/add_app_ttn_ttnmapper9-1.JPG)

Y hasta aquí ya tenemos la parte de configuración en The Things Network, ahora vamos a por la parte física del tracker con todos los componentes.

### Primero los cablecillos

Utilizando módulo LoRa RFM95W sin una placa breadboard  puede ser un poco complicado soldar los cables a los pines de 1,2mm, pero es que a futuro mi objetivo es crear una PCB que lo integre todo.

![](/content/images/2018/12/photo_2018-12-06_23-26-47.jpg)

### Conexión módulo RFM95W al Arduino mini pro

Esquema de las conexiones del módulo RFM95W y del GPS al Arduino mini pro. Todas estas conexiones son necesarias.

![](/content/images/2018/11/esquema-de-conexiones-1.png)

Hay un interesante artículo en <https://primalcortex.wordpress.com/2016/11/25/lpwan-starting-up-with-lorawan-and-the-things-network/> donde he encontrado la información relativa al uso de los pines DIO que tiene el módulo RFM95 y que utiliza la librería LMIC. En resumen viene a ser así:

* **RFM95 (**DI**O0) **–** Arduino (D2):** Indica si hemos recibido datos y si están listos para ser leído por SPI. Indica el fin de la transmisión de datos, que previamente hemos enviado.
* **RFM95 (**DI**O**1**) **–** Arduino (D6):** Recibe timeout del módulo LoRa.
* **RFM95 (**DI**O**2**) **–** Arduino (D7):**Recibe timeout del modo FSK.

Y evidentemente la comunicación por [bus SPI](https://es.wikipedia.org/wiki/Serial_Peripheral_Interface), donde tendremos que relacionar los pines del nodo RFM95 con los pines SPI de Arduino mini pro (***ATENCIÓN**: los pines SPI pueden variar en función del tipo de Arduino que estemos utilizando, consultar documentación*):

* **RFM95 (RESET) - Arduino (D9):** Reset
* **RFM95 (NSS) - Arduino (D10):** Chip Select o Slave Select
* **RFM95 (MOSI) - Arduino (D11):** Master Output Slave Input. Transmisión de datos a otro dispositivo.
* **RFM95 (MISO) - Arduino (D12):** Master Input Slave Output. Es la señal de entrada de nuestro dispositivo desde otro dispositivo.
* **RFM95 (SCK) - Arduino (D13):** Clock. Señal que envía el Master.

A parte de los pines fijos para SPI, el mapeo de los pines que utilizaremos del nodo RFM95W se configuran dentro del código de la siguiente forma en el ***lmic\_pinmap lmic\_pins***:

```auto
    //Pin mapping
    const lmic_pinmap lmic_pins = {
      .nss = 10,
      .rxtx = LMIC_UNUSED_PIN,
      .rst = 9,
      .dio = {2, 6, 7},
    };
```

### Conexión del módulo GPS

La conexión más sencilla es la del GPS. El módulo que he utilizado tienes dos pines de alimentación y otros dos (*TX* y *RX*) para transmisión/recepción de datos UART, que estarán conectamos a dos pines del Arduino. Para ello utilizaremos la librería *SoftwareSerial* y definiremos los pines TX, RX y la velocidad de conexión del GPS:

```auto
    #define RX_GPS 4
    #define TX_GPS 3
    #define BAUD_GPS 9600

    SoftwareSerial gps(RX_GPS, TX_GPS);
```

![](/content/images/2018/12/photo_2018-12-06_23-28-35.jpg)

### El software para el tracker

Comenzamos ahora la parte en la que configuramos nuestro nodo tracker. Para ello vamos a rescatar la información de las *KEYS* de nuestra aplicación definida al principio en el cloud de The Things Network y el identificador del *Devices* para este dispositivo.

![](/content/images/2018/12/Selecci-n_501.png)

La programación del tracker es muy similar a cualquier nodo LoRaWAN utilizando la librería LMIC: a parte de adaptar la configuración anterior, he modificado la función que envía la información **enviar\_datos\_GPS()** , que recoge la información del GPS, genera el payload, lo envía y programa nuevamente el siguiente envío. Pero vamos por partes:

Al inicio del programa he colocado unos parámetros para configurar su comportamiento:

```auto
#define GPS_ON
#define DEBUG_ON
#define MODO_TEST
```

* **GPS\_ON**: Si está definido se leen los datos del GPS y se envían. Hasta que no se obtienen datos fijados del GPS no se pueden enviar. Si no definimos este parámetro (si lo comentamos), en cada intervalo se envía un paquete vacío de datos, esto puede servir si queremos utilizar la aplicación TTNMapper (aunque esto no es el objetivo de este nodo ;) )
* **DEBUG\_ON**: Si está definido muestra por Serial mas información
* **MODO\_TES**T: Si está definido utiliza únicamente el canal 0 para enviar los datos. Sólo para uso de pruebas iniciales:

```auto
#ifdef MODO_TEST
  // --------------------
  // Desactivamos todos los canales menos el canal 0 --> SOLO PARA TESTING!!!
  // Define the single channel and data rate (SF) to use
  // channel = 0   /    DR_SF7
  forceTxSingleChannelDr(0, DR_SF7);
  // ------------------
#endif MODO_TEST
```

Luego tenemos otra serie de definiciones para el módulo GPS: pines de Rx/Tx y velocidad del puerto UART, que ya vimos al principio del artículo:

```auto
#define RX_GPS 4
#define TX_GPS 3
#define BAUD_GPS 9600
```

¿Cada cuanto tiempo enviamos un paquete con la posición GPS?, pues se define en segundos en la siguiente constante *TX\_INTERVAL*:

```auto
const unsigned TX_INTERVAL = 60;
```

No quiero extenderme mucho mas en el artículo, básicamente compartir el código y que podáis verlo y probarlo. Cualquier aclaración, duda o comentario ya sabéis que os podéis poner en contacto conmigo.

**Repositorio GitHub** con el código de ejemplo: <https://github.com/akirasan/LoRaWAN-tracker-TTNmapper>

### Lista de mejoras ya pensadas

1. Añadir **un pulsador para forzar el envío** de un paquete si esperar al intervalo definido.
2. Si en el momento de enviar coordenadas GPS no está fijado el GPS añadir opción de enviar paquete vacío para usar alternativa por aplicación móvil TTNMapper.
3. **PCB** para integrar los componentes y olvidarse de tanto cable (***en proceso***).
4. Aviso por **IFTTT a Telegram**. Esto es sencillo y básicamente es configuración en el cloud de The Things Network. Se trata de añadir una nueva integración con [IFTTT](https://ifttt.com/) (al igual que se ha realizado con TTNmapper) y luego registrar un evento para que desde IFTTT envíe un mensaje a Telegram. Esto es interesante, ya que nos permitirá conocer si alguno de los paquetes que hemos enviado ha llegado a algún gateway. Este punto, realmente **ya lo tengo implementado ;)**
