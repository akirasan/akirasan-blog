---
layout: ../../layouts/post.astro
title: "Emulando Atari 800XL"
pubDate: 2017-04-14
description: "Un frikada de las grandes!!!"
author: "akirasan"
isPinned: false
excerpt: "Un frikada de las grandes!!!"
image:
  src: "/content/images/2017/04/programa_atari800xl-1.png"
  alt: "Emulando Atari 800XL"
tags: ["Linux", "personal"]
---

Un frikada de las grandes!!!

Me he instalado un emulador de un **Atari 800XL** en Linux, únicamente para tratar de programar en **BASIC** el programa que me hizo quedar atrapado con el mundo de la informática desde que tenía 10 años.

Primero vamos con la instalación del emulador. Muy simple para los que tenemos Linux (en mi caso Ubuntu) ya que está en los repositorios y es sencillo de instalar:

`$ sudo apt-get install atari800`

Ahora tenemos que bajarnos las ROMs que permiten cargar el sistema operativo y el interprete de BASIC. Para ello he recurrido a <http://atariarea.krap.pl/PLus/files/xf25.zip>, donde se pueden descargar las ROMs.

Una vez descomprimido el fichero *xf25.zip* en una carpeta, abrimos un terminal y desde esa misma carpeta ejecutamos el comando **atari800**:

![](/content/images/2017/04/Selecci-n_036.png)

Como veréis, no se ve nada, así que hay que maximizar la ventana para poder ver algo.

Bueno hay que configurar el emulador de Atari 800 para que pueda encontrar los ficheros ROMs que hemos descomprimido. Aquí tenéis los pasos. Para entrar en el menú tenéis que pulsar **F1**:

![](/content/images/2017/04/Selecci-n_037.png)  
![](/content/images/2017/04/Selecci-n_040.png)  
![](/content/images/2017/04/Selecci-n_041.png)  
![](/content/images/2017/04/Selecci-n_042.png)  
![](/content/images/2017/04/Selecci-n_043.png)  
![](/content/images/2017/04/Selecci-n_045.png)  
![](/content/images/2017/04/Selecci-n_046.png)  
![](/content/images/2017/04/Selecci-n_044.png)

Una vez configurado y guardada la configuración es recomendable reinicar el emulador. Ahora cuando carga veremos una pantalla nueva: **SELF TEST**, que se utilizaba para testear el sistema, pero nosotros queremos programar en BASIC, así que hay que apretar **F5**.

![](/content/images/2017/04/Selecci-n_047.png)

Y aquí tenemos nuestro esperado **READY**

![](/content/images/2017/04/Selecci-n_048.png)

Para poder recuperar aquel programa he tenido que buscar la documentación de BASIC. Tantos años ya estaba obsoleto!!!. Y he conseguido encontrar ese documento!!! <http://www.atarimania.com/documents/atari-800xl-computer-owners-guide.pdf>. Copiamos el **Program One** y **RUN**

![](/content/images/2017/04/programa_codigo_atari800xl.png)

Y aquí tenéis el resultado del programa que consiguió despertar mi interés por la informática **WOW**

![](/content/images/2017/04/programa_atari800xl.png)

Pues si amigos, con estos tres programas que venían de ejemplo aprendía programar (sin tener ni idea de ingles y sin Internet!!!). Prueba y error hasta conseguir descubrir que hacía cada instrucción.

Ah!!! por cierto, el primer programa **Program One**, ni siquiera lo piqué yo, lo hizo mi hermano...Pero a mi me enganchó tanto esto que no podía parar de pasar horas y horas haciendo cosas.

Mas manuales digitalizados de Atari  
<http://www.atarimania.com/documents-atari-400-800-xl-xe-manuals_2_8.html>
