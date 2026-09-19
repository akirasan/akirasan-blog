---
layout: ../../layouts/post.astro
title: "Enviar coordenadas GPS por LoRaWAN TTN"
pubDate: 2018-10-17
description: "Las coordenadas GPS son números decimales comprendidos en un rango de -90º a 90º para la latitud y de -180º a 180º para la longitud. Los val"
author: "akirasan"
isPinned: false
excerpt: "Las coordenadas GPS son números decimales comprendidos en un rango de -90º a 90º para la latitud y de -180º a 180º para la longitud. Los val"
image:
  src: "/content/images/2018/10/portada_loraGPS-1.png"
  alt: "Enviar coordenadas GPS por LoRaWAN TTN"
tags: ["LoRa", "LoRaWan", "Arduino"]
---

Las coordenadas GPS son números decimales comprendidos en un rango de -90º a 90º para la latitud y de -180º a 180º para la longitud. Los valores negativos/positivos van en función del lado del planeta te encuentres.

Cuando los leemos un dispositivo GPS estas coordenadas suelen venir expresadas en grados decimales con una precisión de 7 decimales.

Uno de los retos que tenemos en la transmisión de información en IoT utilizando LoRaWAN es precisamente la optimización del mensaje que enviamos (cómo ya vimos en el anterior post **[Cómo transmitir datos en LoRaWAN, reduciendo el payload](http://akirasan.net/la-importancia-de-un-buen-payload-en-lorawan/)**). Por lo tanto transmitir un número decimal con signo positivo/negativo puede ser un reto muy común en nuestros dispositivos IoT (coordenadas GPS, temperatura, humedad, radiación solar, etc).

Con un ejemplo práctico, vamos a ver como implementarlo en LoRaWAN. Para ello vamos a conectar un módulo GPS a un nodo LoRa basado en Arduino. Y vamos a codificar y decodificar la información.

## Codificar la información

Cuando leemos nuestro GPS nos devuelve, entre otros, valores para la latitud y la longitud. Imaginemos por ejemplo que la longitud tiene este valor **179,1234567**. Éste valor está dentro del rango de -180º a 180º y con una precisión de 7 decimales.

Sabemos que la información que enviamos a través de LoRaWAN son bytes, por lo tanto vamos a tener que codificar es número decimal a una array de bytes, nuestro payload.

Primero vamos a convertir ese número decimal en un entero. Esto es fácil, para ello simplemente vamos *a correr la coma siete posiciones*, lo multiplicamos por 10.000.000 y nos quedará **un número entero como 1.791.234.567**.

La siguiente pregunta es ¿cuántos bytes necesitamos para almacenar valores como 1.791.234.567?.

### Un poco de teoría de programación

Sabemos que en programación existen varios tipos de datos, los cuales nos permiten reservar espacio de memoria para poder almacenar valores en nuestras variables. Cómo vamos a utilizar un Arduino, debemos fijarnos en los tipos de datos que disponemos para poder almacenar ese valor, incluyendo valores positivos y negativos (perdemos una pequeña parte de memoria, el primer bit determinar el signo del número (1 negativo y 0 positivo)). Por ejemplo:

| Tipo de dato | Tamaño en memoria | Rango de valores |
| --- | --- | --- |
| byte | 1 byte / 8 bits | 0..255 |
| char (con signo) | 1 byte / 8 bits | (7 bits) -128..127 |
| word | 2 bytes / 16 bits | 0.. 65.535 |
| int (con signo) | 2 bytes / 16 bits | (15 bits) -32.768.. 32.767 |
| unsignedlong | 4 bytes / 32 bits | 0.. 4.294.967.295 |
| long | 4 bytes / 32 bits | (31 bits) -2,147,483,648..2,147,483,647 |

Después de este repaso sobre el tipo de datos que podemos utilizar en Arduino, queda claro que para almacenar el número de nuestro ejemplo, el 1.791.234.567, que además podría ser negativo (recordad que la longitud puede ser -180º a 180º), es el tipo **long**, por lo tanto **necesitaremos 4 bytes**.

![](/content/images/2018/10/representacionBIN-3.JPG)

### Troceando byte a byte

¿Cómo enviamos un bloque de 4 bytes en un mensaje de bytes (payload, array de bytes)?. Tenemos que ser capaces de descomponer los 4 bytes que ocupa en memoria nuestro número, en bytes independientes que podamos añadir en el payload. Para hacer esto, debemos bajar un poco mas y **trabajar a nivel de bits**.

La técnica es la siguiente:

1. Aplicar una **operación lógica AND** a los 8 bits (byte) que quiero aislar.
2. **Desplazar esos bits** tantos bits a la derecha como sea posible para formar un byte.
3. **Añadir el byte** al payload.

De una forma visual quedaría así:

**Pasos 1 y 2 (aislar y desplazar)**

![](/content/images/2018/10/byte1_coder-1.JPG)

**Paso 3. Añadimos al payload**

![](/content/images/2018/10/byte1_coder_result-1.JPG)![](/content/images/2018/10/byte1_coder_payload-2.JPG)

Volvemos a aplicar lo mismo para el siguiente byte:

**Pasos 1 y 2 (aislar y desplazar)**

![](/content/images/2018/10/byte2_coder-1.JPG)

**Paso 3. Añadimos al payload**

![](/content/images/2018/10/byte2_coder_result-1.JPG)![](/content/images/2018/10/byte2_coder_payload-1.JPG)

Y así sucesivamente vamos descomponiendo nuestro número almacenado en memoria en bytes.

El código en Arduino quedaría así:

```auto
// [0..3] 4 bytes
payload[0] = (byte) ((longitud_long & 0xFF000000) >> 24 );
payload[1] = (byte) ((longitud_long & 0x00FF0000) >> 16 );
payload[2] = (byte) ((longitud_long & 0x0000FF00) >> 8 );
payload[3] = (byte) ((longitud_long & 0X000000FF));
```

## Descodificar la información

Ya tenemos nuestro payload (o mensaje LoRaWAN) codificado en los 4 bytes que enviamos por LoRa:

![](/content/images/2018/10/bytes_coder_payload-1.JPG)

Una vez el paquete del mensaje se integra en The Things Networks llega el momento de convertir estos bytes en el número inicial que nos dio nuestro GPS. Por lo tanto vamos a tener que **hacer los pasos inversos** para conseguir ese número.

Primero vamos a integrar esos bytes en un solo número. Para ello seguiremos estos pasos (la inversa del envío):

1. Leer un byte del payload
2. Desplazar el byte hasta su posición original (hacia la izquierda).
3. Lo concatenamos al número total.
4. Una vez hemos hecho esto, como la longitud es un número decimal, tenemos que dividirlo por 10.000.000, para recuperar la posición de la coma decimal (recordad, que al inicio lo multiplicamos por éste número).

En LoRaWAN esta tarea se realiza en el función ***Decoder()*** que tiene definida toda aplicación en The Things Networks. El código quedaría así:

```auto
function Decoder(bytes, port) {
      // Decode an uplink message from a buffer
      // (array) of bytes to an object of fields.
    var decoded = {};
    longitud_decode = ((bytes[0]) << 24)
    + ((bytes[1]) << 16)
    + ((bytes[2]) << 8)
    + ((bytes[3]));
    decoded.longitud_gps = longitud_decode / 10000000;
    return decoded;
    }
```

Podemos testearlo, añadiendo nuestro payload en hexadecimal.

![](/content/images/2018/10/codigo_decoder_lorawan-1.png)

**En resumen...**

*Puede parecer complicad*o, pero la clave está en entender el proceso de descomposición y unión de los bytes. Esta técnica no solo la vamos a utilizar para enviar números grandes, sino también **para enviar varios datos en un mismo payload**. Por ejemplo, todos los datos que podemos leer de un GPS: longitud, latitud, satélites, hdop, altitud,...Se trata de ir colocando byte a byte en el payload la información que queremos enviar.

---

***Actualizado [18\_oct\_2018]***. Gracias a **[Màrius Montón](https://twitter.com/mariusmonton)** que se ha dado cuenta de un error en el ejemplo ilustrativo que había utilizado, un error que no afecta al resultado, pero puede parecer raro: **Utilizaba el valor GPS 180,1234567**. Éste valor es imposible, ya que el rango de valores que devuelve un GPS (tal y como explico al inicio del artículo) es de -180º a 180º. Ahora, lo he sustituido por **179,1234567**. Tweet con el aviso: <https://twitter.com/mariusmonton/status/1052828978019532800>
