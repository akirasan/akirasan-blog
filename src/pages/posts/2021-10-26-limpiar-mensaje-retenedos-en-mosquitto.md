---
layout: ../../layouts/post.astro
title: "Limpiar mensajes MQTT retenidos en mosquitto"
pubDate: 2021-10-26
description: "Formas de limpiar mensajes retenidos en el servidor MQTT de *mosquitto* (en Linux)"
author: "akirasan"
isPinned: false
excerpt: "Formas de limpiar mensajes retenidos en el servidor MQTT de *mosquitto* (en Linux)"
image:
  src: "/content/images/2021/10/mqtt_mosquitto_rentenidos.jpg"
  alt: "Limpiar mensajes MQTT retenidos en mosquitto"
tags: ["Linux", "mqtt", "mosquitto"]
---

Formas de limpiar mensajes retenidos en el servidor MQTT de *mosquitto* (en Linux)

Cómo esto es un blog personal, intento apuntar cosillas que luego consulto con el tiempo, y de paso lo comparto. En este caso me apunto/comparto formas de borrar mensajes retenidos en el servidor MQTT *mosquitto*.

### Método 1. Borrar TODOS los mensaje retenidos

***mosquitto*** guarda en una base de datos todos estos mensajes, por lo tanto si la borramos adiós a todos los mensajes. Cómo lo hacemos:

1. Parar el servidor: ***sudo service mosquitto stop***
2. Borrar la base de datos: ***sudo rm /var/lib/mosquitto/mosquitto.db***
3. Arrancamos el servidor: ***sudo service mosquitto start***

### Método 2. Método selectivo

Si lo que queremos es eliminar solamente una sería de mensajes en concreto entonces vamos a tener que ejecutar el comando de publicación mosquitto\_pub con los parámetros siguientes:

***mosquitto\_pub -t el\_topic -n -r -d***

Detalle de lo que hacen estos comandos:

`-n` = Send a null (zero length) message  
`-r` = Retain the message as a “last known good” value on the broker  
`-d` = Enable debug messages
