---
layout: ../../layouts/post.astro
title: "Raspberry generando tweets con Python"
pubDate: 2015-01-21
description: "La comunicación de notificaciones por correo está muy bien, pero ¿porque no hacerse follower de nuestro sistema?. En el futuro con el Intern"
author: "akirasan"
isPinned: false
excerpt: "La comunicación de notificaciones por correo está muy bien, pero ¿porque no hacerse follower de nuestro sistema?. En el futuro con el Intern"
image:
  src: ""
  alt: "Raspberry generando tweets con Python"
tags: ["Linux", "RaspberryPi", "python"]
---

La comunicación de notificaciones por correo está muy bien, pero ¿porque no hacerse follower de nuestro sistema?. En el futuro con el Internet de las Cosas será algo normal.

Vamos a preparar el sistema para generar tweets, lo primero es tener una cuenta de Twitter para nuestro sistema. Yo he preferido que los tweets de esta cuenta sean privados, de esta forma cada follower tiene que ser autorizado para ver los tweets que publiquemos.

Una vez que tenemos cuenta Twitter, es hora de definir una aplicación. Esta definición de aplicación permitirá autenticar mediante OAuth y poder utilizar la API de Twitter.

Aquí os dejo los pasos para crear y conseguir los tokens de acceso que utilizaremos en el código Pyhton. Accedemos a <https://apps.twitter.com/> y creamos nuesta aplicación:

![](/content/images/2015/01/Captura1.JPG)  
![](/content/images/2015/01/Captura2.JPG)  
![](/content/images/2015/01/Captura3.JPG)  
Aquí tenemos los tokens o claves que nos hacen falta para utilizar la API. La *Cosumer Key* y la *Consumer Secret* ya están generadas, pero en la parte inferior tendremos que dar al botón de generar las token:  
![](/content/images/2015/01/Captura4-1.JPG)  
![](/content/images/2015/01/Captura7.JPG)  
Ahora ya tenemos los cuatro parámetros que necesitaremos.

Configuramos el acceso que puede tener la aplicación. Evidentemente tiene que tener lectura/escritura a los tweets  
![](/content/images/2015/01/Captura5.JPG)

Ahora vamos a la parte del sistema. Hay [varias librerías](https://dev.twitter.com/overview/api/twitter-libraries) en python que se pueden utilizar, pero he utilizado el camino sencillo y utilizar la que viene en la distribución, la librería [Tweepy](http://www.tweepy.org/), así que la forma sencilla de instalación es:

```auto
sudo apt-get install python-tweepy
```

Una vez instalado, vamos a generar el primer tweet, para ello necesitaremos las claves y un pequeño código ejemplo como este:

```auto
import tweepy

consumer_key = "<<AQUI_TU_CONSUMER_KEY>>"
consumer_secret = "<<AQUI_TU_CONSUMER_SECRET>>"
access_token = "<<AQUI_TU_ACCESS_TOKEN>>"
access_token_secret = "<<AQUI_TU_ACCESS_TOKEN_SECRET>>"

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)

api = tweepy.API(auth)

tweet = "Hola mundo!!!"
api.update_status(tweet)
```

Pues bien, ahí está el primer tweet!!!  
![](/content/images/2015/01/Captura9.JPG)  
![](/content/images/2015/01/Captura8.JPG)

**UPDATE [05/02/2014]**

Es posible que en algún momento si actualizas la librería Tweepy tengas un error de este tipo:

```auto
tweepy.error.TweepError: [{u'message': u'media_ids parameter is invalid.', u'code': 44}]
```

En tal caso, tienes que nombrar el parámetro en la llamada, es decir, en el ejemplo anterior cambiar esta llamada:

```auto
api.update_status(tweet)
```

Por esta otra:

```auto
api.update_status(status=tweet)
```
