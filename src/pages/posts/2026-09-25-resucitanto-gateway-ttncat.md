---
layout: ../../layouts/post.astro
title: "Resucitando el gateway LoRaWAN de TTN Catalunya: microSD, voltajes y PoE"
pubDate: 2026-09-25
description: "Puesta a punto y solución de problemas de undervoltage en el gateway LoRaWAN que tengo en acogida de TTN Catalunya desde noviembre de 2022."
author: "akirasan"
isPinned: false
excerpt: "Puesta a punto y solución de problemas de undervoltage en el gateway LoRaWAN que tengo en acogida de TTN Catalunya desde noviembre de 2022."
image:
  src: "/images/2026-09-25-ttncat-online-01.webp"
  alt: "Gateway LoRaWAN ttncat-gw32 montado en exterior"
tags: ["LoRa", "LoRaWAN", "TTN", "RaspberryPi", "balenaOS", "DIY", "maker"]
---

<<<<<<< HEAD
=======
Puesta a punto y solución de problemas de undervoltage en el gateway LoRaWAN que tengo en acogida de TTN Catalunya desde noviembre de 2022.

>>>>>>> dbfd332 (update tono post 2026)
Desde el 11 de noviembre de 2022 tengo en acogida, en casa, uno de los gateways comunitarios de [TTN Catalunya](https://ttncat.net/) (el nodo `ttncat-gw32`). Ha estado dando servicio sin quejarse en el tejado, pero hace poco decidió dejar de reportar paquetes y se quedó completamente fuera de juego.

Como esto es un proyecto comunitario y no podemos dejar la cobertura coja, tocó bajar la caja estanca del mástil, meterla en el taller y hacerle una autopsia a fondo.

El gateway está montado sobre una **Raspberry Pi 3B+**, un concentrador LoRaWAN de **RAK Wireless** y corre bajo **balenaOS**, que se encarga de la gestión remota de contenedores. Al abrirlo me encontré con un cóctel de fallos mecánicos y eléctricos que explicaban por qué se había caído.

### Primer tropiezo: la microSD y el zócalo

Al alimentarlo en el banco de trabajo, el LED verde de actividad (ACT) ni parpadeaba. Tras pinchar la tarjeta en un PC, comprobé que las particiones de arranque (`resin-boot`) estaban perfectas.

El fallo era mecánico: el zócalo de la microSD de la Raspberry Pi había perdido presión en las patillas de contacto tras tanto tiempo a la intemperie y con variaciones térmicas. Solo leía la tarjeta si ejercía presión física sobre ella. Tocó sanear las lengüetas con cuidado para devolverle un contacto firme antes de poder continuar.

### El drama eléctrico: The device is undervolted!

Una vez resuelto el arranque, la Raspberry Pi conectó por fin con balenaCloud, pero la consola se inundó de alertas rojas:

> `The device is undervolted! (< 4.65 V)`

<<<<<<< HEAD
El sistema entraba en *throttling* inmediato, la carga de la CPU se disparaba al 93 % y el estado del dispositivo pasaba a `Reduced functionality`, con lo que perdía la conexión de control (`Cloudlink`).

Para aliviar la carga eléctrica mientras buscaba la raíz del problema, apliqué varias directivas de ahorro desde la configuración de balenaOS (`Device Configuration`):

* **Apagado de radios innecesarias:** Añadí `dtoverlay: disable-wifi` y `disable-bt`. Todo el tráfico va por cable Ethernet, así que no tiene sentido alimentar chips de radio extras.
* **Recorte de GPU y HDMI:** Bajé la memoria compartida al mínimo (`gpu_mem=16`) y desactivé la salida de vídeo con `hdmi_blanking=2`.
* **Bajada de frecuencias:** Capé la CPU a 900–1000 MHz y el núcleo a 250 MHz para evitar los picos de demanda instantánea cuando el concentrador LoRaWAN transmite o recibe ráfagas.

Con balenaOS actualizado a la versión **2.115.18** y el Supervisor en orden, el contenedor `udp-packet-forwarder` se mantenía estable en local, pero al volver a montarlo en el tejado, con el cable de red conectado, el *undervoltage* volvía a saltar.
=======
El sistema entraba en *throttling* inmediato, la carga de la CPU se disparaba al 93 % y el estado del dispositivo pasaba a `Reduced functionality`, con lo que perdía la conexión de control (`Cloudlink`). Ains...

Para aliviar la carga eléctrica mientras buscaba la raíz del problema, apliqué varias directivas de ahorro desde la configuración de balenaOS (`Device Configuration`):

* **Apagado de radios innecesarias:** añadí `dtoverlay: disable-wifi` y `disable-bt`. Todo el tráfico va por cable Ethernet, así que no tiene sentido alimentar chips de radio extras.
* **Recorte de GPU y HDMI:** bajé la memoria compartida al mínimo (`gpu_mem=16`) y desactivé la salida de vídeo con `hdmi_blanking=2`.
* **Bajada de frecuencias:** capé la CPU a 900-1000 MHz y el núcleo a 250 MHz para evitar los picos de demanda instantánea cuando el concentrador LoRaWAN transmite o recibe ráfagas.

Con balenaOS actualizado a la versión **2.115.18** y el Supervisor en orden, el contenedor `udp-packet-forwarder` se mantenía estable en local, pero al volver a montarlo en el tejado, con el cable de red conectado, el *undervoltage* volvía a saltar. Vaya faena.
>>>>>>> dbfd332 (update tono post 2026)

### El culpable definitivo: el splitter PoE

La Raspberry Pi se alimenta mediante **PoE (Power over Ethernet)** usando un splitter pasivo/básico. Ahí estaba el cuello de botella:

1. La tirada de cable Ethernet provocaba una caída de tensión considerable.
2. El splitter original apenas entregaba 2 A justos (10 W teóricos). En cuanto el concentrador LoRaWAN demandaba energía en los picos de radio, la línea de 5 V caía por debajo de los 4,65 V críticos de la Raspberry Pi.

<<<<<<< HEAD
La solución definitiva pasó por sustituirlo por un **splitter PoE activo Revotech Gigabit** (norma IEEE 802.3af/at a 48 V), capaz de entregar **5 V y 3 A reales (15 W)** con aislamiento galvánico directamente a la toma de alimentación. Con este cambio, la tensión en la placa no baja de los 4,85 V bajo carga máxima y el aviso de subtensión desapareció al instante.
=======
La solución definitiva pasó por sustituirlo por un **splitter PoE activo Revotech Gigabit** (norma IEEE 802.3af/at a 48 V), capaz de entregar 5 V y 3 A reales (15 W) con aislamiento galvánico directamente a la toma de alimentación. Con este cambio, la tensión en la placa no baja de los 4,85 V bajo carga máxima y el aviso de subtensión desapareció al instante!!!
>>>>>>> dbfd332 (update tono post 2026)

![](/images/2026-09-25-ttncat-online-02.webp)

### De vuelta al mapa de The Things Network

Caja sellada, subida de nuevo al tejado y comprobación en la consola de **The Things Stack**:

![](/images/2026-09-25-ttncat-online-03.webp)

Estado en verde, conexión UDP arriba y sin alertas de hardware.

![](/images/2026-09-25-ttncat-online-04.webp)

Al entrar a ver el tráfico en directo (*Live data*), empezaron a entrar los paquetes de los nodos del entorno: mensajes confirmados, tramas de *Join* y métricas perfectas, con más de 10.000 paquetes gestionados sin una sola pérdida.

<<<<<<< HEAD
Una alegría volver a tener el `ttncat-gw32` resucitado y aportando cobertura a la red de [TTN Catalunya](https://ttncat.net/). ¡Seguimos sumando paquetes en el aire! ;)
=======
Una alegría volver a tener el `ttncat-gw32` resucitado y aportando cobertura a la red de [TTN Catalunya](https://ttncat.net/). ¡Seguimos sumando paquetes en el aire!!! ;)
>>>>>>> dbfd332 (update tono post 2026)
