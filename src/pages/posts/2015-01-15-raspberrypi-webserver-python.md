---
layout: ../../layouts/post.astro
title: "Flask un webserver basado en Python"
pubDate: 2015-01-15
description: "Una forma sencilla de montar un servidor web basado en Python es utilizando . Flask es un framework que permitirá desarrollar/interaccionar"
author: "akirasan"
isPinned: false
excerpt: "Una forma sencilla de montar un servidor web basado en Python es utilizando . Flask es un framework que permitirá desarrollar/interaccionar"
image:
  src: ""
  alt: "Flask un webserver basado en Python"
tags: []
---

Una forma sencilla de montar un servidor web basado en Python es utilizando [Flask](http://flask.pocoo.org/). Flask es un framework que permitirá desarrollar/interaccionar el mundo web con Python. Vamos a ver como funciona:

Primero, evidentemente, lo que tendrémos que hacer es instalar el paquete **Flask**:

```auto
sudo pip install Flask
```

Ahora podemos hacer un pequeño ejemplo para que ver todo funciona correctamente. Editamos un fichero hello.py:

```auto
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

Ahora si accedemos a la dirección de `http://<ip_servidor>:5000`veremos un bonito "Hello World!". A partir de aquí, vamos a desarrollar una aplicación web que nos permita controlar el envío de comandos desde la RPi hacia nuestro Arduino y mejorar el proyecto de domótica.

Para utilizar algo nuevo y sencillo si no queremos complicarnos con el tema del HTML es utilizar [Markdown](http://es.wikipedia.org/wiki/Markdown). Para ello vamos a integrar Markdown con Flask. Lo primero es la instalación:

```auto
easy_install markdown
```

Luego las librerías a utilizar en el código son:

```auto
import markdown
from flask import Flask
from flask import render_template
from flask import Markup
```

Y ahora solo falta implementarlo. [Aquí](http://flask.pocoo.org/snippets/19/) tenéis un ejemplo para empezar a utilizarlo y ver como funciona.
