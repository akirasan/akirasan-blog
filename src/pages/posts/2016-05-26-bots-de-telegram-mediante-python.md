---
layout: ../../layouts/post.astro
title: "Bots de Telegram mediante Python"
pubDate: 2016-05-26
description: "Hace un tiempo  explicaba cómo se podía registrar y desarrollar un bot básico para Telegram. El problema que me he encontrado recientemente"
author: "akirasan"
isPinned: false
excerpt: "Hace un tiempo  explicaba cómo se podía registrar y desarrollar un bot básico para Telegram. El problema que me he encontrado recientemente"
image:
  src: "/content/images/2016/05/python-bot-telegram.png"
  alt: "Bots de Telegram mediante Python"
tags: ["Python"]
---

Hace un tiempo [en este post](http://www.akirasan.net/crear-bots-para-telegram-mediante-python/) explicaba cómo se podía registrar y desarrollar un bot básico para Telegram. El problema que me he encontrado recientemente es que la librería que utilizaba tenía varios problemas, así que he estado revisando y he encontrado algo mas decente y relativamente actualizado. Se trata de la librería [python-telegram-bot](https://python-telegram-bot.org/).

Para su instalación utilizaremos `pip`:

```auto
$ pip install python-telegram-bot
$ python bot.py
```

Vamos a por el código!!!. El tema cambia un poco respecto al de *Telebot*, pero es bastante sencillo de adaptar e implementar. Vamos a ver un ejemplo sencillo.

Vamos a necesitar importar los siguientes componentes:  
`telegram` es obvio y luego vamos a utilizar las extensiones de la clase principal para utilizar `Updater` y `CommandHandler` que nos van a permitir poder gestionar las peticiones de comandos cuando actualicemos el chat

```auto
import telegram
from telegram.ext import Updater
from telegram.ext import CommandHandler
```

Ahora declaramos nuestro bot `mi_bot` y nuestro gestor de actualizaciones `mi_bot_updater`, ambos a partir del TOKEN de autenticación

```auto
TOKEN = '<AQUI_TU_TOKEN_BOT_GENERADO_CON_@BOTFATHER'

#Creamos nuestra instancia "mi_bot" a partir de ese TOKEN
mi_bot = telegram.Bot(token=TOKEN)
mi_bot_updater = Updater(mi_bot.token)
```

Ahora vamos a definir un par de funciones que nos van a servir para gestionar dos comandos:

* **/start**: Que se ejecutará al iniciar una conversación nueva
* **/?**: Que mostrará la ayuda de nuestro bot.

Cómo parámetros a estas dos funciones vamos a pasarles la instancia de nuestro bot y nuestro gestor de actualizaciones `mi_bot_updater`. No sería necesario en estos casos porque por defecto a utilizar la propia instancia, pero creo que así se entiende mejor:

```auto
# Handle para el incio de la conversacion /start
def start(bot=mi_bot, update=mi_bot_updater):
    print "Iniciada conversación: "
    print update.message.chat_id
    bot.sendMessage(chat_id=update.message.chat_id, text="Hola, soy tu bot!!!")

# Handle para la ayuda /?
def ayuda(bot=mi_bot, update=mi_bot_updater):
    print "Solicita ayuda"
    bot.sendMessage(chat_id=update.message.chat_id, text="No tengo mucho que ofrecerte por ahora")
```

Una vez definidas las funciones que van a gestionar nuestros comandos vamos a *"darlas de alta"* en el gestor de comandos:

```auto
#Definimos para cada comando la función que atendera la peticion
start_handler = CommandHandler('start', start)
ayuda_handler = CommandHandler('?', ayuda)
```

Ahora vamos *"a unir"* nuestro gestor de comandos, con nuestro gestor de actualizaciones, para ello utilizamos un *dispatcher*, al cual le vamos a definir nuestros comandos:

```auto
dispatcher = mi_bot_updater.dispatcher

dispatcher.add_handler(start_handler)
dispatcher.add_handler(ayuda_handler)
```

Bien, pues ya estamos casi al final, únicamente hay que lanzar nuestro gestor de actualizaciones del chat y a correr!!!

```auto
mi_bot_updater.start_polling()

while True: #Ejecutamos nuestro programa por siempre
    pass
```

Si vamos a Telegram y buscamos nuestro bot, con este ejemplo tendríamos algo como esto:

![](/content/images/2016/05/Screenshot_20160526-205629.png)  
![](/content/images/2016/05/Screenshot_20160526-205650.png)

~~En la parte del bot y a título informativo, simplemente veremos información cuando se ejecute uno de los comandos ya que en las funciones hemos puesto un `print`~~

Por defecto los métodos de python-telegram-bot no muestran nada por pantalla, así que he puesto algunas salidas por consola para tener cierta traza de lo que va haciendo.
