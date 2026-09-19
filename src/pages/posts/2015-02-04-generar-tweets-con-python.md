---
layout: ../../layouts/post.astro
title: "Generar tweets con una imagen en Python"
pubDate: 2015-02-04
description: "Referente al artículo  para publicar tweets mediante Python, me encontré con el problema de que la librería *Tweetpy* instalada desde reposi"
author: "akirasan"
isPinned: false
excerpt: "Referente al artículo  para publicar tweets mediante Python, me encontré con el problema de que la librería *Tweetpy* instalada desde reposi"
image:
  src: ""
  alt: "Generar tweets con una imagen en Python"
tags: []
---

Referente al artículo [Raspberry generando tweets con Python](http://www.akirasan.net/raspberry-generando-tweets-con-python) para publicar tweets mediante Python, me encontré con el problema de que la librería *Tweetpy* instalada desde repositorio no está del todo actulizada, y claro, si queremos hacer un tweet con una imagen no es posible. En versiones mas actuales de [Tweetpy](http://www.tweepy.org/) tiene un método llamado **update\_\_\_with\_\_\_media** que permite hacer esto.

Nos bajamos la última versión desde <https://github.com/tweepy/tweepy> (ya puede ser con **git** o con un **wget** directamente y luego descomprimimos) y ejecutamos el comando que nos proponen para instalar:

```auto
python setup.py install
```

Si al ejecutar este comando tienes un error como me pasó a mí, te recomiendo que sustituyas el código del fichero `setup.py`:

**ERROR:** al ejecutar el comando de instalación:

```auto
Traceback (most recent call last):
File "setup.py", line 17, in <module>
install_reqs = parse_requirements('requirements.txt', session=uuid.uuid1())
TypeError: parse_requirements() got an unexpected keyword argument 'session'
```

**SOLUCION:** Para solucionarlo, modificamos las líneas siguientes:

```auto
install_reqs = parse_requirements('requirements.txt', session=uuid.uuid1())
reqs = [str(ir.req) for ir in install_reqs]
```

Por estas otras:

```auto
import os
from setuptools import setup

with open('requirements.txt') as f:
   reqs = f.read().splitlines()
```

Al final el problema es que no lee correctamente el fichero con los requerimientos. Ahora si ejecutamos el comando de instalación anterior no debería salir ningún error.

Tras el proceso de instalación y si todas las dependencias y requerimientos de otras librerías se cumplen ya podemos utilizar el método de twittear con una imagen, por ejemplo **la detección de movimiento en casa**:

```auto
    import tweepy

    ## Datos de acceso cuenta Twitter
    consumer_key="<AQUI_TU_CONSUMER_KEY>"
    consumer_secret="<AQUI_TU_CONSUMER_SECRET>"
    access_token="<AQUI_TU_TOKEN>"
    access_token_secret="<AQUI_TU_TOKEN_SECRET>"

    f_imagen="/tmp/motion/lastsnap.jpg"

    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    auth.secure = True

    api = tweepy.API(auth)

    tweet = "Captura de movimiento"
    api.update_with_media(f_imagen,status=tweet)
```

**Resultado:** Un bonito tweet con una imagen incluida:  
![](/content/images/2015/02/tweet_captura_python.png)

###### Actualizado [11/05/2015]

Un error conocido en ciertas plataformas con Python (específicamente, versiones de Python inferiores a la 2.7.9) tienen restricciones en el uso del módulo SSL, lo que causa que peticiones HTTPS fallen. El error que obtenemos es un warning de este tipo:

```auto
/usr/local/lib/python2.7/dist-packages/requests/packages/urllib3/util/ssl_.py:90: InsecurePlatformWarning: A true SSLC.
  InsecurePlatformWarning
```

Se recomienda actualizar a una versión de Python superior o usar pyOpenSSL. Vamos a hacer esta última recomendación:  
`sudo pip install pyOpenssl`
