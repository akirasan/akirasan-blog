---
layout: ../../layouts/post.astro
title: "Conectar y comunicar módulo Bluetooth + Arduino"
pubDate: 2016-07-05
description: "Esta entrada es un poco a lo loco!!!. Recientemente me han pasado una consulta sobre conexión y comunicación entre un Arduino Mega y una shi"
author: "akirasan"
isPinned: false
excerpt: "Esta entrada es un poco a lo loco!!!. Recientemente me han pasado una consulta sobre conexión y comunicación entre un Arduino Mega y una shi"
image:
  src: "/content/images/2016/07/IMG_20160705_210102_20160705_210411-2.jpg"
  alt: "Conectar y comunicar módulo Bluetooth + Arduino"
tags: ["Arduino"]
---

Esta entrada es un poco a lo loco!!!. Recientemente me han pasado una consulta sobre conexión y comunicación entre un Arduino Mega y una shield con un módulo Bluetooth. En tema es que yo hace tiempo hice algo con Bluetooth, pero es que ahora me he pasado al módulos con WiFi :). Total, que por ayudar que no quede!!!

Cómo no tenía ninguno de esos módulos Bluetooth ya preparados para conexión con Arduino, encontré en casa un módulo *"en bruto"* así que tocó hacer un poco de *cirugía de guerra* para hacerlo funcionar.

![](/content/images/2016/07/IMG_20160705_210102_20160705_210411.jpg)

Ya veis la pinta que tiene,...se aguanta con un celo. El *pinout* de estos módulos es éste (en función del modelo, el mío es un HC-05):

![](/content/images/2016/07/pinout_bluetooth.jpg)

Únicamente he conectado los pines necesarios: Rx, Tx, KEY, Vcc y GND, nada mas!!!. Y conectados a un Arduino UNO

![](/content/images/2016/07/IMG_20160705_204009_20160705_210413.jpg)

![](/content/images/2016/07/IMG_20160705_203951_20160705_210415.jpg)

#### Primeros pasos

###### Configurar módulo BT con comandos AT

Lo primero que tenemos que hacer para que funcione nuestro módulo cómo nosotros queremos (a parte de conectarlo correctamente a nuestra placa Arduino) es configurar sus parámetros, para ello es necesario que acepte **[comandos AT](https://es.wikipedia.org/wiki/Conjunto_de_comandos_Hayes)**, para ello tenemos que poner el pin denominado *KEY* a *HIGH*, es decir, con voltaje.

```auto
  pinMode(KEY_PIN, OUTPUT);
  digitalWrite(KEY_PIN, HIGH); delay(500);
```

Una vez tenemos esto comenzamos a configurar nuestro módulo. En el código de ejemplo que os dejaré mas abajo, ejecuto pocas instrucciones y básicamente configuramos el nombre de nuestro dispositivo `test_arduino` (todo el código está disponible en mi GitHub)

```auto
void setup()
{
  // Entramos en modo comandos AT para configurar nuestro módulo bluetooth
  pinMode(KEY_PIN, OUTPUT);
  digitalWrite(KEY_PIN, HIGH); delay(500);

  BT.begin(9600);     //Velocidad del puerto del módulo Bluetooth
  Serial.begin(9600); //Abrimos la comunicación serie con el PC y establecemos velocidad

  Serial.println("Configuracion AT");
  enviar_comando_AT("AT");

  Serial.println("Version:");
  enviar_comando_AT("AT+VERSION");

  Serial.println("Nombre dispositivo:");
  enviar_comando_AT("AT+NAME=test_arduino");

  //Cerramos el modo comandos AT
  digitalWrite(KEY_PIN, LOW); delay(500);

}
```

###### Preparar escucha

Una vez tengo configurado el módulo BT con los comandos AT, comienzo a el `loop()` con simplemente escuchar por el puerto serie establecido con el módulo BT y Arduino y volcar la información por el `Serial` para ver lo que me ha llegado.

```auto
void loop()
{
  if (BT.available())
  {
    Serial.write(BT.read());
  }

  if (Serial.available())
  {
    BT.write(Serial.read());
  }
}
```

Arrancando este programa, tendremos algo similar a esto en la salida del puerto serie de Arduino:

![](/content/images/2016/07/at_comand_bt_arduino_1.jpg)

#### Establecer conexión y comunicarse

###### Establecer conexión

Yo he utilizado mi móvil Android para conectarme a la señal que emite el módulo Bluetooth

![](/content/images/2016/07/Screenshot_20160705-204510_20160705_210827.jpg)

Cómo en mi configuración de comandos AT no he especificado el comando `AT+PIN`por defecto el PIN de conexión es 0000

![](/content/images/2016/07/Screenshot_20160705-204517_20160705_210808.jpg)

###### Enviar datos por Bluetooth

Para enviar dados desde mi móvil Android he utilizado una aplicación que permite enviar por *Serial Bluetooth*. He utilizado [BlueTooth Serial Controller](https://play.google.com/store/apps/details?id=nextprototypes.BTSerialController), pero podéis utilizar cualquier otra aplicación.

![](/content/images/2016/07/Screenshot_20160705-204401_20160705_210807.jpg)

Una vez instalada, conectamos a nuestro dispositivo Bluetooth `test_arduino`:

![](/content/images/2016/07/Screenshot_20160705-204530_20160705_210826.jpg)

Ahora desde la app enviamos un mensaje *Hola mundo!!!*, y vemos como nuestro módulo recibe al instante:

**Envío**  
![](/content/images/2016/07/Screenshot_20160705-204549_20160705_210808.jpg)

**Recepción**  
![](/content/images/2016/07/at_comand_bt_arduino_2.jpg)

Cómo verlo en *screenshot* es un poco feo, he grabado un pequeño vídeo donde se puede ver mejor.

#### El código completo

El *sketch* de Arduino lo podéis encontrar en mi canal de GitHub <https://github.com/akirasan/arduino_bluetooth>

Recordar de modificar en el código los pines que utilizáis en vuestro proyecto.

Se que me falta mucha información, pero cómo he comentado al inicio ha sido una post del estilo pim-pam. Creo que por lo menos puede ayudar a focalizar el inicio en la comunicación con Arduino + Bluetooth.
