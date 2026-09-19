---
layout: ../../layouts/post.astro
title: "Monitoriza tu velocidad de conexión con Python + Plot.ly"
pubDate: 2014-02-06
description: "En un tweet the Linuxparty leo una forma de comprobar la velocidad de la conexión de internet desde la línea de comandos en Linux utilizando"
author: "akirasan"
isPinned: false
excerpt: "En un tweet the Linuxparty leo una forma de comprobar la velocidad de la conexión de internet desde la línea de comandos en Linux utilizando"
image:
  src: ""
  alt: "Monitoriza tu velocidad de conexión con Python + Plot.ly"
tags: ["Internet", "Linux", "plotly", "python"]
---

En un tweet the Linuxparty leo una forma de comprobar la velocidad de la conexión de internet desde la línea de comandos en Linux utilizando Python:

> Comprobar la velocidad de Internet desde la línea de comandos en Linux: Cuando experimenta una lenta conexión ... <http://t.co/PHGZ9qKy2m>
>
> — linuxparty (@linuxparty) [February 3, 2014](https://twitter.com/linuxparty/statuses/430249784587194369)

Partiendo de esta base y viendo la posibilidad de probar la API de Plot.ly (servicio para crear gráficas) para Python, me he creado un pequeño script que ejecuto cada 5 minutos (por ahora, seguramente lo deje en una ejecución cada hora), que verifica la velocidad de la conexión y alimenta una gráfica en Plot.ly.

Evidentemente lo primero es crear una cuenta en [Plot.ly](https://plot.ly "https://plot.ly") para poder tener de un usuario y una key para utilizar con la API (en el código de mi script está puesto como *<USUARIO>* y *<KEY>*

Después en nuestro sistema tenemos que instalar la librería Python para instalar la API de Plot.ly:

```auto
$# sudo apt-get install python-pip
$# sudo pip install plotly
```

Ahora nos bajamos el script para verificar la velocidad de conexión que sale [en el artículo de Linuxparty](http://www.linux-party.com/index.php/29-internet/9035-comprobar-la-velocidad-de-internet-desde-la-linea-de-comandos-en-linux# "http://www.linux-party.com/index.php/29-internet/9035-comprobar-la-velocidad-de-internet-desde-la-linea-de-comandos-en-linux#"):

```auto
wget https://raw.github.com/sivel/speedtest-cli/master/speedtest_cli.py
```

Con esto ya tendremos todos los elementos.

```auto
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# by Akirasan Febrero 2014

import plotly.plotly as py
import datetime
import speedtest_cli
import sys
import StringIO
import string

from datetime import datetime

py = py.sing_in("<USUARIO>", "<KEY>")

#guardamos y redireccionamos el sys.stdout
stdout = sys.stdout
sys.stdout = reportSIO = StringIO.StringIO()

#Llamamos a la utilidad speedtest_cli
speedtest_cli.speedtest()

#Recuperamos lo escrito en nuestro IO
reportStr = reportSIO.getvalue()
#recuperar sys.stdout
sys.stdout = stdout

#Buscamos posición de la información
dl_pos = string.find(reportStr, 'Download:')
ul_pos = string.find(reportStr, 'Upload:')

dia = datetime.today()

layout = {'title': 'Velocidad conexion internet'}
data = [{'x': dia,
         'y': reportStr[dl_pos+9:dl_pos+9+6],
         'name':'Download'
         },
        {'x': dia,
         'y': reportStr[ul_pos+7:ul_pos+7+6],
         'name':'Upload'
         }
        ]

py.plot(data, filename='Velocidad_Internet', fileopt='extend', world_readable=False, layout=layout)
```

Luego ya solo falta o ejecutarlo como un script poniendo los permisos de ejecución y programarlo con el *cron* o similar.

Una vez hemos alimentado la información en Plot.ly podemos ir a consultar y ver el resultado de la gráfica. Algo como esto:

![](/content/images/2015/01/qtrnas-1e3dbc_1024x1024.jpg)
