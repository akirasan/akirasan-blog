---
layout: ../../layouts/post.astro
title: "Crear Bots para Telegram mediante Python"
pubDate: 2015-07-03
description: "---"
author: "akirasan"
isPinned: false
excerpt: "---"
image:
  src: "/content/images/2015/09/telegram_logo.png"
  alt: "Crear Bots para Telegram mediante Python"
tags: []
---

---

**ACTUALIZADO**: Yo he tenido problemas con esta librería de este estilo:

```auto
AttributeError: 'TeleBot' object has no attribute 'message_handler'
```

**No he conseguido solucionarlo**, así que he me puesto a buscar una alternativa, aquí os dejo otra entrada en el blog: <http://www.akirasan.net/bots-de-telegram-mediante-python/>

---

Post original

Enviar mensajes Telegram en Pyhton (o linea de comandos en linux) ya era posible mediante [Telegram CLI for Linux](https://github.com/vysheng/tg), pero ahora [podemos crear bots](https://telegram.org/blog/bot-revolution) mediante al API de Telegram, haciendo que se comuniquen con los servidores de Telegram mediante HTTP y JSON. Para utilizar la API de Telegram y no morir en el intento, existen algunas alternativas como [pyTelegramBotAPI](https://github.com/eternnoir/pyTelegramBotAPI) (**telebot**) para Python.

La instalación la podemos hacer mediante *git* o *pip*. Aquí los dos métodos:

```auto
git clone https://github.com/eternnoir/pyTelegramBotAPI.git
cd pyTelegramBotAPI
sudo python setup.py install
```

O *pip*:  
`sudo pip install pyTelegramBotAPI`

Para poder utilizar esta API, primero vamos a tener que regristar nuestro bot y disponer del *token* que nos proporciona el bot llamado [The BotFather](https://telegram.me/botfather). Es muy simple, únicamente hay que abrir conversación desde Telegram con @BotFather y seguir los pasos: una vez tenemos abierto chat con @botfather, enviamos el siguiente mensaje para crear nuestro bot: **/newbot** y *The BotFather* nos preguntará por el nombre de nuestro bot (nombre que será visible) y el nombre de usuario (un nombre corto entre 5-32 caracteres y que **debe acabar** con la palabra **bot**).

![alt](/content/images/2015/07/botfather_entry.JPG)

Con todo esto The BotFather nos contestará con un string que será nuestro TOKEN y utilizaremos en nuestra programación Python para identificarnos.

![alt](/content/images/2015/07/botfather.JPG)

**The BotFather** tienes otros comandos de edición una vez creado: **/setname**, **/setdescription**, **/setabouttext**, **/setuserpic**, **/setcommands**, **/setjoingroups**, **/setprivacy**, **/deletebot**.

Ahora vamos a ver algunos ejemplos de los métodos que podemos utilizar con esta API en Python. Lo primero es el import de la librería y la definición de la conexión mediante el string TOKEN:

```auto
import telebot

TOKEN = '<token string>' #Ponemos nuestro TOKEN generado con el @BotFather

mi_bot = telebot.TeleBot(TOKEN) #Creamos nuestra instancia "mi_bot" a partir de ese TOKEN
```

Siempre que queramos enviar algo a un chat, va a ser necesario conocer el chat\_id, es decir, el identificador del chat con el que estamos o queremos interactuar. Vamos a ver un sencillo ejemplo para empezar a tener las nociones básicas: Un bot que únicamente va a repetir aquello que le digamos:

```auto
import telebot

TOKEN = '<string TOKEN>' #Ponemos nuestro TOKEN generado con el @BotFather
mi_bot = telebot.TeleBot(TOKEN) 						#Creamos nuestra instancia "mi_bot" a partir de ese TOKEN

def listener(*mensajes):  ##Cuando llega un mensaje se ejecuta esta función
    for m in mensajes:
        chat_id = m.chat.id
        if m.content_type == 'text':
            text = m.text
            mi_bot.send_message(chat_id,"Me copio de tu texto")
            mi_bot.send_message(chat_id, text)

mi_bot.set_update_listener(listener) #registrar la funcion listener
mi_bot.polling()

while True: #No terminamos nuestro programa
    pass
```

El resultado de este script será el siguiente:  
![alt](/content/images/2015/07/bot_telegram.JPG)

Ahora ya solo queda comenzar a darle un poco de inteligencia y que sea capaz de interpretar y entender algunas acciones.

Como guía, aunque tenéis todos los métodos explicados en [pyTelegramBotAPI](https://github.com/eternnoir/pyTelegramBotAPI), aquí os dejo algunos métodos para ir empezando:

Enviar un simple mensaje a un chat  
`mi_bot.send_message(chat_id, text)`

Enviar una foto:

```auto
photo = open('/tmp/mifoto.jpg', 'rb')
mi_bot.send_photo(chat_id, photo)
```

Enviar un archivo (por ejemplo un pdf, doc, xls,...):

```auto
doc = open('/tmp/documento.pdf', 'rb')
mi_bot.send_document(chat_id, doc)
```

Enviar un vídeo:

```auto
video = open('/tmp/timelapse.mp4', 'rb')
mi_bot.send_video(chat_id, video)
```

Enviar un fichero de audio:

```auto
audio = open('/tmp/sound.mp3', 'rb')
mi_bot.send_audio(chat_id, audio)
```

Enviar un Sticker (si tienes uno creado):

```auto
sticker1 = open('/tmp/mi_sticker.webp', 'rb')
mi_bot.send_sticker(chat_id, sticker1)
```

**ACTUALIZADO**: Yo he tenido problemas con esta librería de este estilo:

```auto
AttributeError: 'TeleBot' object has no attribute 'message_handler'
```

No he conseguido solucionarlo, así que he me puesto a buscar una alternativa, aquí os dejo otra entrada en el blog: <http://www.akirasan.net/bots-de-telegram-mediante-python/>
