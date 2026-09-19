---
layout: ../../layouts/post.astro
title: "Keen.IO sistema de reporting de eventos"
pubDate: 2015-02-12
description: "es una de esas herramientas que nos puede servir para analizar datos dentro del mundo de IoT (). Por un lado, mediante su API permite regist"
author: "akirasan"
isPinned: false
excerpt: "es una de esas herramientas que nos puede servir para analizar datos dentro del mundo de IoT (). Por un lado, mediante su API permite regist"
image:
  src: ""
  alt: "Keen.IO sistema de reporting de eventos"
tags: ["Python", "IoT"]
---

[Keen IO](https://keen.io/) es una de esas herramientas que nos puede servir para analizar datos dentro del mundo de IoT ([Internet Of Things](http://es.wikipedia.org/wiki/Internet_de_las_cosas)). Por un lado, mediante su API permite registras eventos que nosotros mismos podemos crear, y por otro lado, nos proporciona una herramienta para poder explotar la información que hemos ido generando y analizarla. Por ejemplo, registrar la entrada de un usuario, la temperatura de nuestra CPU, recoger datos de un sensor,...todo lo que queramos monitorizar y generar un evento que posteriormente podamos analizar. Es decir, podemos ver a Keen IO como una plataforma donde almacenar nuestros datos.

![keen flow](/content/images/2015/02/keen_flow2.png)

Puedes visitar su página para entenderlo mejor, pero tal vez con un ejemplo sea mas práctico.

Para empezar nos tendremos que registrar y generar nuestro primer proyecto. Con el cual tendremos unas KEY necesarias para utilizar la API (**WRITE KEY**, **READ KEY**, **MASTER API KEY**)

![](/content/images/2015/02/keen_crear_proyecto.JPG)

Ahora vamos a nuestro sistema. Para variar lo voy a hacer en Python (últimamente no paro de hacer cositas con Python, pero es que es realmente sencillo):

Necesitamos instalar la librería keen, para ello ejecutaremos el comando de instalación `pip install keen`, pero previamente, ya que es un requisito, tenemos que instalar la librería **pycrypto**, que es un requisito previo para instalar Keen:

```auto
sudo apt-get install python-crypto
```

Ahora instalamos tal cual nos indican en [Github](https://github.com/keenlabs/KeenClient-Python)

```auto
sudo pip install keen
```

El momento de la verdad, un ejemplo muy sencillo: Imaginemos un sensor te temperatura/humedad que nos envía cada 10 minutos una medición:

```auto
import keen
from keen.client import KeenClient

client = KeenClient(
        project_id="<AQUI_TU_PROJECT_ID>",
        write_key="<AQUI_TU_WRITE_KEY>",
        read_key="<AQUI_TU_READ_KEY>
#       master_key="<AQUI_TU_MASTER_KEY>"
    )

client.add_event("sensor1", {
        "temperatura": "12.1",
        "humedad": "78.0"
    })
```

*Yo he tenido problemas instanciando el cliente con el parámetro MASTER\_KEY, que en la documentación aparece, así que he optado por eliminarlo (comentado en el código).*

Está claro que el código anterior *"no funciona"* como un sensor, simplemente **sirve para simular** lo que haría el sensor. Para ello cambiamos manualmente los valores de "temperatura" y "humedad" y ejecutamos el código, así generaremos un pequeño volumen de datos para probar y ver como registra los datos en Keen IO.

Una vez hemos alimentado Keen IO, vamos a ver la información grabada. Podemos ver los eventos grabados y su información:

![](/content/images/2015/02/keen_datos_proyecto.JPG)

También podemos ver la estructura, donde aparecen para el evento *"sensor1"*, los campos que hemos enviado: *"humedad"* y *"temperatura"*, pero también existen tres campos mas que Keen IO añade para ayudar a identificar el momento del evento.

![](/content/images/2015/02/keen_estructura_datos.JPG)

Ahora podemos en la parte de Workbench simular queries sobre los datos y sacar algunos indicadores.

![](/content/images/2015/02/keen_query_proyecto.JPG)

Esta interface nos servirá luego mas adelante para poder diseñar nuestro Dashboard con la información que queremos. Pero eso será en otro episodio,...
