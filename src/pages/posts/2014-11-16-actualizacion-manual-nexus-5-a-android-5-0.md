---
layout: ../../layouts/post.astro
title: "Nexus 5. Actualización manual a Android 5.0"
pubDate: 2014-11-16
description: "¿Eres un impaciente como yo y no puedes esperar a que te llegue la actualización a la nueva versión?, pues sigue estos pasos y actualizala d"
author: "akirasan"
isPinned: false
excerpt: "¿Eres un impaciente como yo y no puedes esperar a que te llegue la actualización a la nueva versión?, pues sigue estos pasos y actualizala d"
image:
  src: ""
  alt: "Nexus 5. Actualización manual a Android 5.0"
tags: []
---

¿Eres un impaciente como yo y no puedes esperar a que te llegue la actualización a la nueva versión?, pues sigue estos pasos y actualizala de forma manual.

Para empezar necesitaremos los siguientes ingredientes:

* **Paquete Android SDK** que podéis encontrar en <http://developer.android.com/sdk/index.html>, que descomprimiremos e instalaremos el **Google USB Driver** para Android abriendo el **SDK Manager**  
  ![](/content/images/2014/11/sdkmanager.JPG)
* El **fichero OTA para Nexus 5**. Aquí es importante entender para otros navegantes, que tiene que ser el fichero OTA no la imagen de fábrica ni otras histórias. Es importante, ya que vamos a simular el mismo proceso de actualización de OTA pero de forma manual. Evitando la perdida de configuración (recomendado siempre tener copia de seguridad de fotos y archivos importantes):

4.4.4 (KTU84P) -> [5.0: hammerhead LRX21O from KTU84P](http://android.clients.google.com/packages/ota/google_hammerhead/c1a33561be84a8a6a7d5a4c8e3463c4db9352ce6.signed-hammerhead-LRX21O-from-KTU84P.c1a33561.zip)  
4.4.4 (KTU84Q) -> [5.0: hammerhead LRX21O form KTU84Q](http://android.clients.google.com/packages/ota/google_hammerhead/67fdc56df808024ba5ebd95d4e16358c3b4f96cb.signed-hammerhead-LRX21O-from-KTU84Q.67fdc56d.zip)

* En nuestros Nexus 5 tendrémos que tener activada la opción de Debuggin USB (Ajustes-> pulsamos unas 8 veces para activar el modo Desarrollador y poder activar el modo debug USB)

Una vez tenemos los ingredientes vamos al lio!!!

* Movemos el fichero OTA ZIP en la carpeta donde hemos descomprimido el Android SDK, por ejemplo, en *C:\temp\Android\_SDK*\**sdk\platform-tools\**
* Abrimos un terminal MS-DOS y vamos al path anterior (en mi ejemplo *C:\temp\Android\_SDK*\**sdk\platform-tools\**) donde encontraremos la herramienta **adb** que mas adelante utilizaremos
* Apagamos el teléfono y entramos en **Recovery mode** de la siguiente forma:

  + Apretamos y mantemos apretados a la vez botón de volumen bajo y encendido hasta que entremos en el **bootloader**. Con los botones de volumen buscamos la opción de **Recovery mode** y pulsamos el botón de Encendido

  ![](/content/images/2014/11/image006.png)

  + Una vez en *Recovery mode* pulsando el boton de volumen + veremos un menú donde seleccionaremos **apply update from ADB** y pulsaremos el botón de encendido para seleccionar.
  + Conectamos por USB al ordenador. Aquí es importante que nos reconozca el dispositivo y el driver anteriormente instalado sea utilizado correctamente. En caso negativo, tendrémos que entrar en el Administrador de dispositivos y actualizar el driver (el driver lo podrémos encontrar en *C:\temp\Android\_SDK*\**sdk\extras\google\usb\_driver\android\_winusb.inf\**)
  + Cuando nuestro dispositivo es reconocido sin problemas por Windows, nos vamos al terminal que teníamos abierto y ejecutamos el comando: **adb sideload < fichero OTA ZIP >** el cual comenzará a enviar el paquete al teléfono. Si el teléfono no es correctamente detectado por el driver, el comando **adb** no detectará ningún terminal conectado y no se ejecutará.  
    ![](/content/images/2014/11/IMG_20141116_012639.jpg)
  + Llegados a este punto, solo falta ir viendo como se va actualizando progresivamente, hasta que el teléfono volverá al **Recovery mode**:  
    ![](/content/images/2014/11/IMG_20141116_012548.jpg)  
    ![](/content/images/2014/11/IMG_20141116_013045.jpg)
  + Una vez hemos vuelto al **Recovery mode** reiniciamos el teléfono, el cual tardará en ir actualizando las app's que tengamos instaladas.  
    ![](/content/images/2014/11/IMG_20141116_013135.jpg)  
    ![](/content/images/2014/11/IMG_20141116_013443.jpg)  
    ![](/content/images/2014/11/IMG_20141116_014903.jpg)

He de reconocer que al principio se hace algo raro, pero supongo que será cosa de acostumbrarse.

![](/content/images/2014/11/IMG_20141116_015018.jpg)
