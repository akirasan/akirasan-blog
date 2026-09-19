---
layout: ../../layouts/post.astro
title: "MiniGateway LoRa monocanal con ESP32"
pubDate: 2018-05-12
description: "Vamos a ver los pasos para convertir nuestro pequeño módulo ESP32+LoRa en un gateway LoRa monocanal. Esto quiere decir que únicamente va a s"
author: "akirasan"
isPinned: false
excerpt: "Vamos a ver los pasos para convertir nuestro pequeño módulo ESP32+LoRa en un gateway LoRa monocanal. Esto quiere decir que únicamente va a s"
image:
  src: "/content/images/2018/05/IMG_20180512_012108.jpg"
  alt: "MiniGateway LoRa monocanal con ESP32"
tags: ["LoRa", "LoRaWan", "Arduino", "DIY", "ESP32", "IoT"]
---

Vamos a ver los pasos para convertir nuestro pequeño módulo ESP32+LoRa en un gateway LoRa monocanal. Esto quiere decir que únicamente va a ser capaz de escuchar y enviar información por el único canal que tiene configurado. Esto limita la red pública.

Pero antes recordad de tener preparado el IDE de Arduino para soporte al módulo ESP32. En este post anterior explico cómo se puede realizar **GHOST\_URL**/preparando-arduino-ide-para-esp32-lora/. Sin esta configuración no se podrá seguir los siguientes pasos.

Vuelvo a recomendar **la estupenda guia de [Bricolabs](https://bricolabs.cc/wiki/guias/lora_ttn)** específica para estos tipos de módulos.

## Configuración del gateway

Siguiendo los pasos de la guia Bricolabs, vamos a necesitar hacer ciertas modificaciones a nivel de hardware antes de pasar a cargar el software.  
Empezamos bajando la librería de [Marteen Westenberg](https://github.com/kersing) que tiene publicado en su repositorio GitHub <https://github.com/kersing/ESP-1ch-Gateway-v5.0>. Descomprimos todo y para no tener muchos problemas con la cantidad de librerías que necesita, yo he optado por copiarlo todo en mi carpeta de usuario Arduino:

![Selecci-n_161](/content/images/2018/05/Selecci-n_161.png)

En la carpeta de *libraries* moveis todo a vuestra carpeta personal de *libraries* dentro de Arduino:

![Selecci-n_162](/content/images/2018/05/Selecci-n_162.png)

![Selecci-n_164](/content/images/2018/05/Selecci-n_164.png)

Ahora copiamos la carpeta *ESP-sc\_gway* con el código necesario y el fichero *ESP-sc-gway.ino* que cargaremos en nuestro IDE de Arduino.

![Selecci-n_165](/content/images/2018/05/Selecci-n_165.png)

Veremos que nos ha cargado bastantes archivos, todos necesarios para el proyecto.

![Selecci-n_168](/content/images/2018/05/Selecci-n_168.png)

![Selecci-n_170](/content/images/2018/05/Selecci-n_170.png)

El archivo de configuración **ESP-sc-gway.h** nos va permitir configurar diferentes conceptos interesantes:

El servidor TTN al que nos conectaremos. Por defecto lo vamos a dejarlo así, utilizando los propios de *The Things Network*:

![Selecci-n_171](/content/images/2018/05/Selecci-n_171.png)

Definición de características de nuestro gateway. Por defecto está así:  
![Selecci-n_172](/content/images/2018/05/Selecci-n_172.png)

Revisamos que el ***spreading factor*** esté definido como **SF7**, es importante cuando utilizamos un único canal de comunicación. El *spreading factor* especifica la potencia de transmisión, la subfrecuencia y el tiempo de aire (*Time on Air*).  
![Selecci-n_175](/content/images/2018/05/Selecci-n_175.png)

Aquí podéis ver un gráfico de como el SF se define el factor. Hay que tener en cuenta que a mas distancia mas consumo de energía.

![Selecci-n_176](/content/images/2018/05/Selecci-n_176.png)

Cambiamos a "1" el valor **\_STRICT\_1CH** para indicar que solamente utilizaremos el primer canal.

![Selecci-n_177](/content/images/2018/05/Selecci-n_177.png)

Definimos el servidor *ntp* de horario, vamos a poner un local.

![Selecci-n_178](/content/images/2018/05/Selecci-n_178.png)

```auto
#define NTP_TIMESERVER "es.pool.ntp.org"
```

Hacemos cambios en la descripción, donde es interesante poner que es un ESP32, la frecuencia y el *spreading factor* (SF7) que hemos definido anteriormente. La posición geográfica y email de contacto:

```auto
// Gateway Ident definitions
#define _DESCRIPTION "ESP32 Gateway 868.1Mhz SF7"
#define _EMAIL "akirasan@mi.com"
#define _PLATFORM "ESP32"
#define _LAT 42.506151
#define _LON 2.256303
#define _ALT 2
```

Velocidad de conexión al puerto serie, en este caso 115200. Y una opción que yo he deshabilitado, ya que mi módulo no lleva pantalla OLED: **OLED 1** pasa a **OLED 0**.

![Selecci-n_173](/content/images/2018/05/Selecci-n_173.png)

Lleva una de las partes mas importantes **configuración de la conexión WiFi**. En esta sección hay que hacer dos cambios.

![Selecci-n_174](/content/images/2018/05/Selecci-n_174.png)

Forzar el *if* a que sea verdadero siempre, eso se hace cambiando el 0 por un 1. Y luego poner el nombre de nuestra Wifi y contraseña. Ojo!!! que el primer elemento *{ "" , "" }* no se debe tocar. Quería como algo así:

```auto
#if 1
wpas wpa[] = {
  { "" , "" },					// Reserved for WiFi Manager
  { "MiWifi", "123456_Passwd" },
  { "MiWifi_5G", "123456_Passwd" }
};
#else
// Place outside version control to avoid the risk of commiting it to github ;-)
#include "d:\arduino\wpa.h"
#endif
```

Una vez tenemos realizados estos cambios de la configuración, compilamos y subimos a nuestro módulo ESP3. Revisamos entonces lo que nos muestra por el puerto *Serial*:

![Selecci-n_179](/content/images/2018/05/Selecci-n_179.png)

La información que aparece es muy importante, porque podemos ver si se ha conectado a nuestra WiFi y el código del **Gateway ID**. Un identificador que necesitamos para registrarlo en TTN.

Pero no te preocupes, porque tu ESP32 está emitiendo un pequeño servidor web donde te mostrará toda la información.

Si quieres conocer la IP que tiene tu nuevo *gateway* (para poder acceder vía web), puedes conectarte a tu router y ver la dirección IP que se le ha asignado por DHCP, o puedes usar esta modificación que he propuesto. Modificar el fichero **\_wwwServer** y añadir estas dos líneas para que cuando arranque, puedas ver por la consola del *Serial* la dirección IP asignada dentro de tu red WiFi.

```auto
 Serial.print(F("IP :"));
 Serial.println(WiFi.localIP());
```

![Selecci-n_180](/content/images/2018/05/Selecci-n_180.png)

Y este es el resultado cuando nos conectamos a nuestro gateway vía web:

![Selecci-n_181](/content/images/2018/05/Selecci-n_181.png)

## Registro en The Things Networks

Llega el momento de dirigirnos a la página de [The Thing Networks](https://www.thethingsnetwork.org/) y crea una cuenta, si no la tenemos ya. Cuando estás logado, accede a tu consola para poder **configurar un nuevo gateway**.

![Selecci-n_182](/content/images/2018/05/Selecci-n_182.png)

![Selecci-n_183](/content/images/2018/05/Selecci-n_183.png)

Informamos los diferentes campos que nos piden y aquí lo importante es primer campo ***Gateway EUI***, y es el número de nuestro **gateway ID** y que podemos consultar desde la web del ESP32.

Y si todo ha ido bien, tendrás tu gateway listo para recibir paquetes LoRa!!!

![Selecci-n_185](/content/images/2018/05/Selecci-n_185.png)

Hasta aquí esta segunda parte sobre la configuración de dispositivos LoRa. El siguiente paso: configurar un nodo LoRa para emitir información hacia una aplicación definida en TTN, através de nuestro Gateway. Pero eso será en otro post ;)
