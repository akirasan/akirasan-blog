---
layout: ../../layouts/post.astro
title: "Trasteando con Tasker"
pubDate: 2014-10-30
description: ", una aplicación para automatizar prácticamente cualquier cosa dentro de nuestro Android. Tiene una gran variedad de condiciones y eventos q"
author: "akirasan"
isPinned: false
excerpt: ", una aplicación para automatizar prácticamente cualquier cosa dentro de nuestro Android. Tiene una gran variedad de condiciones y eventos q"
image:
  src: "/content/images/2014/10/nexusae0_tasker.png"
  alt: "Trasteando con Tasker"
tags: []
---

[Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm), una aplicación para automatizar prácticamente cualquier cosa dentro de nuestro Android. Tiene una gran variedad de condiciones y eventos que podemos ir adaptando. La versión para descargar en Google Play cuesta 2,99€, pero podéis [descargar una versión trial](http://tasker.dinglisch.net/download.html) en la página web del desarrollo.

He comenzado a trastear un poco y preparar algunas automatizaciones que aquí os explico. Ojo!! que son muy sencillas y muy básicas, es una primera toma de contacto:

* **Apagar y encender la Wifi a una hora determinada**, prácticamente cuando estoy fuera de casa en el trabajo.
* **Activar Bluetooth cuando ejecuto una aplicación** y apagarlo cuando salgo.

Dos ejemplos muy sencillos y útiles para empezar.

### Apagar y encencer la Wifi a una hora determinada

Entre las 8:00 y las 17:00 de lunes a viernes, lo normal es que esté trabajando fuera de casa sin conexión Wifi. Por lo tanto ¿para que utilizarla?¿para consumir batería?. Pues con esta configuración la apago a las 8:00 y la enciendo a las 17:00, y además como es un ejemplo, pongo un pop-up informativo.

Lo primero que tenemos que hacer es definir un *Contexto*, es decir, en que condiciones queremos que se ejecuten la/s tarea/s. Defino dos Contextos o Perfiles: uno que sean las 17:00h y otro que sean las 8:00h, y a cada uno le asigno una tarea *Encender WIFI* o *Apagar WIFI*:

![](/content/images/2014/10/Screenshot_2014-10-29-21-20-29.png)  
![](/content/images/2014/10/Screenshot_2014-10-29-21-20-35.png)

Dentro de la tarea **Encender WIFI** primero condiciono la ejecución de las  
subtareas o subprocesos a que el día de la semana (representado por la variable global **%DAYW**) sea de Lunes a Viernes:

![](/content/images/2014/10/Screenshot_2014-10-29-21-20-56.png)  
![](/content/images/2014/10/Screenshot_2014-10-29-21-21-00.png)

Como he comentado previa la activación/desactivación de la Wifi, muestro un pop-up con un mensaje avisando de la ejecución, para ello defino una acción del tipo *Alerta* de esta forma:

![](/content/images/2014/10/Screenshot_2014-10-29-21-21-08-1-.png)

La otra acción de *encender Wifi* se realiza con una acción del tipo *Red*. La verdad es que hay gran varidad de acciones, pero Tasker tiene un buscador que nos facilita. Si en el filtro ponemos *wifi* ya nos muestra que cosas podemos hacer y detectar.

Lo mismo haremos con la tarea de *Apagar WIFI*, pero esta vez con el contexto de que sean las 8:00 de la mañana.

### Activar Bluetooth al iniciar una aplicación y desactivarlo al cerrarla

Tengo una de esas smartband [**Vidonn 2.0**](http://www.vidonn.com/en/) que se sincronizan con el móvil vía Bluethoot. Pero normalmente no tengo el BT activado, así que cada vez que quiero sincronizar la smartband tengo que activar el BT, arrancar la aplicación y cuando he finalizado apagar el BT, para ahorro de energía. Con Tasker esto se puede automatizar de tal forma que creamos dos tareas: una para encender el bluethoot y otra para apagarlo. Y las asociamos a un *Contexto o Perfil* de Aplicación, de tal forma que podemos indicar que hacer cuando la aplicación se inicia y que tarea hacer cuando la aplicación finaliza.

![](/content/images/2014/10/Screenshot_2014-10-29-21-20-40.png)

Estos ejemplos son muy sencillos, pero Tasker es mucho mas pontente (podemos definir interface de usuario para interactuar, enviar mensajes, detectar notificaciones y llamadas, etc,...) y versátil, así que seguiré *trasteando* un poco mas.
