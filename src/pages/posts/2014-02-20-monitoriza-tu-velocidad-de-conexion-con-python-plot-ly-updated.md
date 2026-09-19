---
layout: ../../layouts/post.astro
title: "Monitoriza tu velocidad de conexión"
pubDate: 2014-02-20
description: "Después de llevar varios días monitorizando mi conexión de internet mediante mi script, y realizando variaciones y comprobaciones, he llegad"
author: "akirasan"
isPinned: false
excerpt: "Después de llevar varios días monitorizando mi conexión de internet mediante mi script, y realizando variaciones y comprobaciones, he llegad"
image:
  src: ""
  alt: "Monitoriza tu velocidad de conexión"
tags: ["Linux", "python"]
---

Después de llevar varios días monitorizando mi conexión de internet mediante mi script, y realizando variaciones y comprobaciones, he llegado a la conclusión que la mejor forma de utilizar el speedtest-cli es fijando siempre la prueba contra unos servidores. De esta forma no dejamos que speedtest-cli determine el servidor contra el cual hacer la prueba de velocidad y que puede desvirtualizar el resultado real del estado de la conexión.

Para ello creo que lo mejor es utilizar dos o tres servidores y hacer la prueba siempre sobre estos. Así que he modificado el script para que permita configurar una lista de servidores. Para conocer la lista de servidores (el número unívoco que utilizar speedtest-cli), es tan sencillo como llamar al script de *speedtest-cli.py* con el parámetro `--list`. Aparecerán los servidores que por ubicación son preferentes para hacer la prueba de conexión. Selecciona un par y configura el script a tu gusto.

También he modificado la forma de invocar a *speedtest\_cli.py*, para poder pasarle parámetros. Ahora hay que indicar la ruta absoluta de donde está el script.

```auto
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# by Akirasan Febrero 2014

import plotly
import speedtest_cli
import string
import subprocess
from datetime import datetime

dia = datetime.today()
py = plotly.plotly(username_or_email="<USUARIO>", key="<KEY>")
layout = {'title': 'Velocidad Fibra'}

#Lista de servidores contra los que realizar test
#list_servidores = ['3747','4374','3466','1695','3171']
list_servidores = ['1695','2052']

for server in list_servidores:
  reportStr = subprocess.check_output(["python", "<PATH_ABSOLUTO>/speedtest_cli.py","--server",server])

  #Buscamos posición de la información
  dl_pos = string.find(reportStr, 'Download:')
  ul_pos = string.find(reportStr, 'Upload:')
  srv_pos1 = string.find(reportStr,'Hosted by')
  srv_pos2 = string.find(reportStr,'[')
  offset = srv_pos2 - srv_pos1
  servidor = reportStr[srv_pos1+10:srv_pos1+offset]

  data = [{'x': dia,
           'y': reportStr[dl_pos+9:dl_pos+9+6],
           'name':servidor+'_DL'
           },
          {'x': dia,
           'y': reportStr[ul_pos+7:ul_pos+7+6],
           'name':servidor+'_UL'
           }
          ]

  try:
     py.plot(data, filename='Monitorizacion Fibra Optica', fileopt='extend')
  except:
     print "ERROR envio datos a Monitorizacion"
```

El resultado son gráficas en las que se puede ver, que cuando el test de velocidad para un servidor da unos ratios muy malos, para otro se sigue manteniendo estable:  
![](/content/images/2015/01/FO_velocidad_compare.JPG)

Os dejo un ejemplo en tiempo real [https://plot.ly/~akirasan/4](https://plot.ly/~akirasan/4 "https://plot.ly/~akirasan/4")
