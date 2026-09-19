---
layout: ../../layouts/post.astro
title: "Programar módulo wifi ESP8266 con el IDE de Arduino"
pubDate: 2015-09-17
description: "Estoy metido en la creación de sensores mediante el **módulo ESP8266** integrados con Arduino (todo a nivel personal, claro). Actualmente te"
author: "akirasan"
isPinned: false
excerpt: "Estoy metido en la creación de sensores mediante el **módulo ESP8266** integrados con Arduino (todo a nivel personal, claro). Actualmente te"
image:
  src: "/content/images/2015/09/IMG_20150917_233238.jpg"
  alt: "Programar módulo wifi ESP8266 con el IDE de Arduino"
tags: ["IoT", "Arduino", "ESP8266"]
---

Estoy metido en la creación de sensores mediante el **módulo ESP8266** integrados con Arduino (todo a nivel personal, claro). Actualmente tengo un sistema para monitorizar y controlar la calefacción de casa basado en la comunicación por **nRF24L01**, pero los estoy pasando a ESP8266, que creo que son más versátiles y simplifican la comunicación (protocolo), ya que podemos utilizar nuestra wifi que está por toda la casa. Además se está creando una extensa comunidad de IoT (Internet Of Thing) que se focaliza en este nodo, por lo que podemos encontrar mucha información, e incluso la unión del IDE de Arduino con el módulo ESP8266 en [arduinesp.com](http://www.arduinesp.com/)

Bueno vamos al lío: el objetivo va a ser encender y apagar un relé conectado a la caldera (bueno puede estar en una caldera o en cualquier otro aparato que queremos utilizar). Ahora mi nodo utiliza un Arduino mas un módulo RF nrf24l01 y un relé. Vamos a migrar este hardware!!!

###### Antes

[![antes_rele_nrf24lantes_rele_nrf24l01_arduino](/content/images/2015/09/IMG_20150824_152545.jpg)  
[![antes_rele_nrf24lantes_rele_nrf24l01_arduino](/content/images/2015/09/IMG_20150824_152556.jpg)

###### Después

[![despues_rele_esp8despues_rele_esp826](/content/images/2015/09/IMG_20150825_163416.jpg)  
[![despues_rele_esp8despues_rele_esp826](/content/images/2015/09/IMG_20150825_163402.jpg)

### Software

Para comenzar, vamos a por el software. Ingredientes:

* **IDE Arduino**, versión 1.6.5 (recomendable)
* Instalar el soporte para los nodos ESP8266. Desde la opción de **Preferencias**, pondremos en el campo: **Additional board managers URL**, la URL <http://arduino.esp8266.com/stable/package_esp8266com_index.json>. Desde esta URL vamos a descargar el soporte del módulo ESP8266 (podemos añadir más de una, separando las urls por comas)
* Cargaremos luego la definición desde el **Boards Manager** que se encuentra en el menú de **Herramientas**.

[![ide\_arduino1](htt![ide_arduino1](/content/images/2015/09/Selecci-n_046.jpg#small)](/content/images/2015/09/Selecci-n_046.jpg)  
[![ide\_arduino2](htt![ide_arduino2](/content/images/2015/09/Selecci-n_047.jpg#small)](/content/images/2015/09/Selecci-n_047.jpg)  
[![ide\_arduino3](htt![ide_arduino3](/content/images/2015/09/Selecci-n_048.jpg#small)](/content/images/2015/09/Selecci-n_048.jpg)  
[![ide\_arduino4](htt![ide_arduino4](/content/images/2015/09/Selecci-n_049.jpg#small)](/content/images/2015/09/Selecci-n_049.jpg)

Si tienes cualquier duda en el proceso, en <http://www.arduinesp.com/getting-started> tienes mas detalle.

### Hardware

Lo sencillo ya está, ahora vamos al hardware:

* Módulo ESP8266 (en cualquiera de sus sabores, yo he elegido el ESP8266-01, ya que para el propósito de uso es uno de los puertos GPIO que tiene)
* Una placa USB-TTL para comunión serie por USB (si puede ser que trabaje a 3.3v, sino os vais a cargar el ESP8266!!!, recuerda que solo puede ir a 3.3v). Y utilizo un modelo que puedes variar el voltaje de 3.3v o 5v:  
  [![USB-TTL](http://a![USB-TTL](/content/images/2015/08/usb-ttl-ft232rl-pinout.png)](/content/images/2015/08/usb-ttl-ft232rl-pinout.png)
* Una placa de programación DIY. Si, yo me he hecho una para simplificar...Es muy sencilla, y nos servirá para poder programar y testear más fácilmente nuestro ESP8266.  
  [![Placa-DIY-ESP8266Placa-DIY-ESP8266-USB-TTL](/content/images/2015/09/IMG_20150821_172646.jpg)

La placa consiste en conectar RX y TX del USB-TTL con sus respectivos contrarios TX y RX de ESP8266. Es recomendable tener un pulsador, ya que para programar el módulo ESP8266 vamos a tener que poner el pin de GPIO0 a GND para que durante el boot, entre en modo *"upload"* y poder *"tostarle"* nuestro programa.

Para testear tu placa, te aconsejo que utilices uno de los ejemplos que vienen en la librería, como el típico *"hola mundo"*, que normalmente consiste en hacer un *blink* del led que lleva incorporado el módulo ESP8266.

Para programar nuestro módulo vamos a tener que seguir siempre estos pasos con nuestra placa de programación:

1. **Desconectar el módulo ESP8266** de nuestra placa de programación.
2. Pulsar nuestro interruptor para **colocar el pin GPIO0 a GND**.
3. **Conectar nuestro módulo ESP8266** en nuestra placa de programación. Al tener el pin GPIO0 a GND entrará en modo *"programación"*.
4. Desde nuestro IDE de programación **hacemos el *"upload"* de nuestro skecth**. Veremos como se comunica.
5. Una vez ha finalizado el upload, **sacamos de nuestra placa de programación el módulo ESP8266**.
6. Volvemos a tocar el interruptor del **pin GPIO0 y lo dejamos abierto**, de tal forma que no esté conectado a GND.
7. **Volvemos a conectar nuestro módulo ESP8266** sobre la placa de programación y el boot comenzará a ejecutar nuestro programa.

### Programación "arduino"

Con todos los componentes listos (IDE+placa de programación+módulo ESP8266), vamos a programar un sencillo ejemplo [podéis descargar desde aquí](http://akirasan.net/content/images/2015/09/sensor_SWITCH_esp8266.ino).

Comenzamos declarando las librerías que vamos a necesitar `ESP8266WiFi.h` y `ESP8266WebServer.h`. El nombre de nuestra wifi y la password para poder conectarnos.

```auto
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

const char* ssid = "<<SSID_WIFI>>";
const char* password = "<<PASSWORD_WIFI>>";

const int PIN_RELE = 2;   //Es el puerto GPIO2
String ESTADO_RELE = "OFF";

ESP8266WebServer server(80);
```

Por último con `ESP8266WebServer server(80);` definimos un servidor http que va atender peticiones por el puerto standard 80.

Las siguientes funciones: `info()` y `estado_rele()`, nos servirán para contestar a las peticiones http. Simplemente nos devolverá información y estado del relé. Las funciones `on_rele()` y `off_rele()`, para activar y desactivar el relé. Y por último tenemos la función `no_encontrado()`, que nos servirá cuando nos llegue una petición incorrecta por http:

```auto
void info() {
  String info_mensaje = "<b>ESP8266</b><br>Comando <b>on</b>: encender<br>Comando <b>off</b>: apagar<br><br>";
  String str_estado = ESTADO_RELE;
  info_mensaje = info_mensaje + "Estado:" + str_estado + "<br>";
  server.send(200, "text/html", info_mensaje);
}

void estado_rele() {
  server.send(200, "text/plain", ESTADO_RELE);
}

void no_encontrado() {
  server.send(404, "text/plain", "ERROR EN LA PETICION :( ");
}

void on_rele() {
  server.send(200, "text/plain", "OK ON");
  ESTADO_RELE = "ON";
  digitalWrite(PIN_RELE, LOW);
}

void off_rele() {
  server.send(200, "text/plain", "OK OFF");
  ESTADO_RELE = "OFF";
  digitalWrite(PIN_RELE, HIGH);
}
```

Entramos en el `setup()`. A destacar simplemente `WiFi.begin(ssid, password);`, que nos permite conectar a nuestra wifi.

```auto
void setup(void) {
  pinMode(PIN_RELE, OUTPUT);
  digitalWrite(PIN_RELE, HIGH);

  Serial.begin(115200);
  WiFi.begin(ssid, password);
  Serial.println("");

  // Esperamos a que se conecte
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.print("Conectedo a ");
  Serial.println(ssid);
  Serial.print("Dirección IP: ");
  Serial.println(WiFi.localIP());

  IPAddress sensor_ip = WiFi.localIP();
```

Vamos a declarar en el servidor los diferentes paths que vamos a atender y reconocer. Por ejemplo, con `server.on("/", info);` definimos que en cuando nos mandan una petición http a la raíz, llamaremos a la función `info()` que hemos definido anteriormente y nos va a sacar información sobre nuestro módulo (bueno, la que hemos definido nosotros claro).

```auto
  // Declaramos un par de paths por si queremos consultar directamente el estado
  server.on("/", info);
  server.on("/estado", estado_rele);
```

Y así sucesivamente definimos las peticiones de encendido y apagado del relé:

```auto
  // Declaramos los paths para el control del rele
  server.on("/on", on_rele);
  server.on("/off", off_rele);
```

El método `server.onNotFound()` definiremos la función que queremos que actúe cuando la petición que recibimos no la tenemos definida. En este caso, vamos a llamar a la función `no_encontrado()` que hemos definido anteriormente.

```auto
  server.onNotFound(no_encontrado);

  server.begin();
  Serial.println("Servidor HTTP activo");
}
```

El `loop()` es de lo mas simple, simplemente invocar al método `handleClient()` que estará escuchando las peticiones del servidor.

```auto
void loop(void) {
  server.handleClient();
}
```

### Testeando nuestro interruptor remoto

Una vez cargado correctamente mediante el IDE de Arduino el programa anterior en el módulo ESP8266 (siguiendo el procedimiento que hemos comentado antes: **hay que poner el pin GPIO0 a GND** para que el boot del módulo entre en modo *"upload"*).

![](/content/images/2015/09/Selecci-n_050.jpg#small)

Abrimos un navegador y ponemos la dirección IP que se le ha asignado al módulo. En mi caso la *10.2.0.208*. Vemos que al indicar la raíz nos muestra la información que hemos definido en la función `info()`

![](/content/images/2015/09/Selecci-n_051.jpg#small)

![](/content/images/2015/09/IMG_20150917_231044.jpg#small)

Ahora si modificamos la URL con */on* le diremos que active el pin GPIO2, o lo que es lo mismo la activación del relé, según la función `on_rele()`.

![](/content/images/2015/09/Selecci-n_052.jpg#small)

Y lo contrario si le mandamos la petición */off*

![](/content/images/2015/09/Selecci-n_053.jpg#small)  
Pues ya tenemos nuestro interruptor remoto mediante wifi.
