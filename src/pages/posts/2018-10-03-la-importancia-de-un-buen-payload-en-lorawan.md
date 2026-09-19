---
layout: ../../layouts/post.astro
title: "Cómo transmitir datos en LoRaWAN, reduciendo el payload"
pubDate: 2018-10-03
description: "Si estáis comenzando con  y más concretamente con el proyecto de  os habréis dado cuenta (o estaréis a punto de hacerlo) que la información"
author: "akirasan"
isPinned: false
excerpt: "Si estáis comenzando con  y más concretamente con el proyecto de  os habréis dado cuenta (o estaréis a punto de hacerlo) que la información"
image:
  src: "/content/images/2018/10/payload.jpg"
  alt: "Cómo transmitir datos en LoRaWAN, reduciendo el payload"
tags: ["LoRa", "LoRaWan"]
---

Si estáis comenzando con [LoRaWAN](https://www.thethingsnetwork.org/docs/lorawan/) y más concretamente con el proyecto de [The Things Networks](https://www.thethingsnetwork.org/) os habréis dado cuenta (o estaréis a punto de hacerlo) que la información que se transmite por el aire debe estar lo más optimizada posible para reducir al máximo lo que se conoce como *“time on air”* o *“airtime”* (tiempo en el aire), es decir, el tiempo que necesitamos para transmitir nuestro mensaje desde el nodo emisor hasta el receptor (*gateway*). A más tiempo, más saturamos la frecuencia y a más tiempo también más consumo de energía. Por lo tanto es algo en lo que tenemos que pensar para que nuestro proyecto, la red TTN y la tecnología LoRaWan sea sostenible. Por ejemplo, para encender la calefacción a distancia, con un simple “1” sería suficiente para hacerlo.

Realmente hay serie de límites en la política de uso de TTN, un *fairplay* que limita los datos que puede enviar cada dispositivo al día, y que se resume en:

* Una media de 30 segundos de *airtime* (tiempo de aire) en el uplink (envío de datos a TTN).
* Un máximo de 10 mensajes de *downlink* (recepción de datos desde TTN), incluyendo los ACK’s (mensajes de confirmación).

Por lo tanto, es muy importante mantener un mensaje o **payload** lo más reducido posible y con intervalos de varios minutos entre cada envío. **Adicionalmente hay que pensar que el protocolo de LoRaWAN añade 13 bytes a nuestro mensaje** O\_o.

Realmente hay mucha información al respecto de los límites de uso de LoRaWAN y a parte de la propia regulación de las frecuencias europeas, **EU 863-870MHz ISM**, que **limita el uso al 1% para los datos**.

En definitiva, tenemos que ser muy finos con el envío de los datos y por lo tanto es importante prestar atención en este aspecto. Y aunque parezca que 30 segundos de airtime en total al día es muy poco, debemos pensar que nuestros mensajes tardan milisegundos en ser enviados y que es una red pensada para dispositivos IoT (Internet of Things).

#### ¿Y cómo se transmite la información?

Para enviar datos, tanto de ida (*uplink*) cómo de vuelta (*downlink*) a través de The Things Network, se usan bytes. Es decir, el mensaje que se envía es un churro de bytes (*array*) que contiene nuestra información. Por lo tanto, hay una serie de conceptos y limitaciones que tenemos que tener claros.

Vamos a ver **un ejemplo de lo que NO se tiene que hacer** y entenderéis exactamente la importancia de todo esto.

Ejemplo: El clásico termostato inalámbrico que mide la temperatura y decide en cada momento cuando encender y apagar la calefacción.

Imaginemos que nuestro nodo TTN basado en Arduino tiene un sensor de temperatura y cuando detecta que hace frio, envía un mensaje para encender la calefacción. El código mediante LMIC podría ser:

```auto
unsigned uint8_t mensaje[] = "encender";
LMIC_setTxData2(1, mensaje, sizeof(mensaje)-1, 0);
```

Cuando enviamos nuestro paquete de datos, estamos enviando:

![](/content/images/2018/10/envio-mensaje1.JPG)

21 bytes: 8 bytes -> mensaje y 13 bytes -> Protocolo LoRaWAN

Vale, tal vez no incumplamos con los límites establecidos en el uso de LoRa y LoRaWAN, pero seguro que lo podemos mejorar. Mmmm,…y si le damos una vuelta? ¿cómo reducir el mensaje y por lo tanto su airtime y consumo energético?, fácil, no? y si reducimos el texto:

```auto
unsigned uint8_t mensaje[] = "ON";
```

Nuestro paquete pasará a:

![](/content/images/2018/10/envio-mensaje2.JPG)

15 bytes: 2 bytes -> mensaje y 13 bytes -> Protocolo LoRaWAN

¿Se puede mejorar?, claro que si!!!, si pensamos en valores numéricos y el encendido puede ser un ‘1’ y apagado un ‘0’:

```auto
byte mensaje[] = 1;
```

Nuestro paquete pasará a:

![](/content/images/2018/10/envio-mensaje3.JPG)

14 bytes: 1 bytes -> mensaje y 13 bytes -> Protocolo LoRaWAN

Es un ejemplo muy chorra y sencillo, pero la idea es que debemos pensar en la importancia de esto. Y sobretodo en **el envío de texto**, algo que realmente **consume muchos, muchos bytes!!!**. Por esa razón hay que establecer un protocolo de comunicación propio. Veamos otro ejemplo:

Supongamos que tenemos un nodo al que le hemos conectado tres sensores de movimiento: uno en la cocina, otro en el comedor y otro el baño. La idea es enviar información de las habitaciones que están ocupadas. ¿Podrías enviar la información del estado de todas las habitaciones en un solo mensaje de 1 byte (+13 bytes del protocolo LoRaWAN)?

Como sabemos que 1 byte son 8 bits, podríamos establecer que cada posición de un bit es el estado de una habitación, por ejemplo:

![](/content/images/2018/10/codigo-habitaciones.JPG)

![](/content/images/2018/10/codigo-habitacion2.JPG)

De esta forma un 00000101 estaríamos indicando que el baño y la cocina están ocupados. **Enviaríamos 1 byte con el valor en decimal 5**.

Con esta *“técnica”*, podrías saber en un mismo mensaje el estado de ocho sensores en modo de activado/desactivado o controlar por ejemplo 8 relés para activar/desactivar ocho dispositivos domóticos: luces, caldera, la cafetera,…

Hasta aquí todo bien, ¿pero que pasa si esa información la queremos enviar por *HTTP* desde TTN a un servicio web que solo acepta *JSON*?¿cómo transformamos o conectamos nuestra información de LoRaWAN con otros servicios o con nuestras aplicaciones?. Para estos casos nos fijaremos en el script *Decoder()*, el cual se define a nivel de la aplicación dentro de TTN, en el apartado *Payload Formats*.

![](/content/images/2018/10/pantalla-localizar-decoder.JPG)

Éste script se ejecuta automáticamente para cada paquete de entrada que recibe nuestra aplicación definida en TTN, y se utiliza para transformar la información que hemos *codificado* en origen en algo *mas entendible*.

Pero antes de pasar y ver un ejemplo, hay que tener en cuenta tres cosas cuando utilizamos estos scripts estándars de TTN (seguro que hay más, pero estas son para empezar a entrar en materia):

1. El lenguaje de programación es **JavaScript**
2. La información del **payload se muestra en hexadecimal**, por lo tanto en lugar los posibles valores que tiene un byte y que van de 0 a 255, se verán en hexadecimal de 00 a FF. Es cuestión de tenerlo claro.
3. **Podemos simular** la llegada de un payload (eso si en formato HEX hexadecimal).
4. El resultado de la función *Decoder()* **se devuelve en formato JSON**.  
   Pues teniendo más o menos claro esto, podemos hacer una rutina como la siguiente:

![](/content/images/2018/10/c-digo-decoder.JPG)

Esto permite ver el valor recibido de una forma *más amigable* y entendible a simple vista, y además nos permite enviar ese JSON o adaptarlo para otro servicio web.

![](/content/images/2018/10/resultado-decoder.JPG)

#### ¿Y qué pasa con los otros tipos de datos mas complejos?

Hemos visto que para enviar información a través de LoRaWAN utilizamos un array de bytes, 1 byte son 8 bits, y 1 byte puede contener un valor de 0 a 255 en decimal, de 00000000 a 11111111 en binario o de 00 a FF en hexadecimal,…entonces ¿qué pasa con los números mas grandes de 255, con los números negativos o con los decimales?. Bien, para todos ellos hay una solución y en The Things Network hay un artículo donde nos explican que podemos trabajar únicamente con bytes para enviar cualquier valor numérico, aquí os dejo el link [Working with Bytes](https://www.thethingsnetwork.org/docs/devices/bytes.html).

**En resumen:** este post está orientado a concienciar en que debemos prestar atención en el consumo que supone el envío y recepción de datos a través de LoRa y LoRaWAN, y así poder seguir creciendo sosteniblemente esta red IoT. Esto solo lo conseguiremos si entre todos cuidamos de ella.

**Actualización 22/06/2019**: Aquí os dejo link a la presentación sobre el tema que he realizado: [TTN-Optimizacion Payload.pdf](https://github.com/akirasan/TTN_LoRaWAN/blob/master/TTN%20-%20Optimizacion%20Payload.pdf)
