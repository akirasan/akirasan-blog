---
layout: ../../layouts/post.astro
title: "Instalación de Node-RED en Raspi3"
pubDate: 2018-02-11
description: "He de reconocer que la instalación e información sobre como instalar *Node-RED* está muy, pero que muy bien explicada en su página <https://"
author: "akirasan"
isPinned: false
excerpt: "He de reconocer que la instalación e información sobre como instalar *Node-RED* está muy, pero que muy bien explicada en su página <https://"
image:
  src: "/content/images/2018/02/image4168-2.png"
  alt: "Instalación de Node-RED en Raspi3"
tags: []
---

He de reconocer que la instalación e información sobre como instalar *Node-RED* está muy, pero que muy bien explicada en su página <https://nodered.org/> y la verdad es que poco se puede aportar mas.

Hay que tener en cuenta que *Node-RED* funciona bajo el motor [Node.js](https://nodejs.org/). Básicamente, para aquellos que no conozcan que es *Nodejs*, es un servidor capaz de ejecutar código javascript.

He elegido la instalación de *Node-RED* bajo una Raspberry Pi 3, ya que el objetivo es tenerla encendida 24x7 haciendo tareas de,...yo que se...pero va a ser divertido tener un sistema como *Node-RED* tan intiutivo y sencillo de controlar.

### ...pero antes: Prueba en directo

Antes de instalar en una Raspi3, hice la prueba sobre un equipo con Ubuntu Desktop y la verdad es que resultó realmente fácil. Además lo he llamado *Prueba en directo* porque precisamente estaba viendo el *Friking Friday Show* de Oscar de [@BricoGeek](https://twitter.com/bricotienda), dónde explicaba como había utilizado *Node-RED* y *MQTT* para monitorizar la temperatura/humedad y encender y apagar un led con una sesión abierta desde el móvil. Ahí me recordó que hacia tiempo quería probar *Node-RED*,...y venga, mientras veía sus frikadas en directo, me lo instalaba en el PC. Aquí os dejo el link del [Friking Friday Show 09/02/2018 - NodeRed, MQTT y Raspberry Pi](https://www.youtube.com/watch?v=OQvPD9Pxjp8)

![](/content/images/2018/02/upload-1518313764.jpg)

Primero tenemos que instalar *Nodejs*, plataforma sobre la que está desarrollado *Node-RED*.

`sudo apt install nodejs`

Una vez instalado Nodejs ya podemos utilizar su gestor de paquetes, llamado **npm**, para instalar *Node-RED*. Un comando más y listo!!!

`sudo npm install -g --unsafe-perm node-red`

Ahora simplemente ejecutamos y ya tenemos nuestro servidor *Node-RED* corriendo y accesible sobre la IP local 127.0.0.1 y puerto 1880. Así que abrimos un navegador y colocamos eso.

![Selecci-n_082](/content/images/2018/02/Selecci-n_082.jpg)

### Raspberry Pi 3: Raspbian + Nodejs + Node-RED

Desempolvado una Raspberry Pi...

![](/content/images/2018/02/upload-1518337654.jpg)

Vamos a utilizar la distribución de **Raspbian LITE**, la cual **no lleva por defecto *Node-RED* instalado** y tampoco un escritorio, así lo hacemos mas divertido y además conseguimos que nuestro sistema consumirá menos recursos, ya lo voy a orientar a ser un servidor.

Si por el contrario utilizáis la distribución **Raspbian con DESKTOP** ahí **ya tenéis Node-RED instalado por defecto**. Así que puedes ir directamente al apartado de arranque.

Voy a saltarme la instalación de la distribución de Raspbian LITE en SD, ya que existe ya una guías para hacerlo, y la verdad es que son cuatro pasos. Si alguno no sabe como hacerlo tenéis os dejo [aquí el enlace a la guía de instalación](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) donde podréis encontrar varias formas en función del sistema operativo que utilizais.

<https://www.raspberrypi.org/downloads/raspbian/>  
![raspbian-distro-lite](/content/images/2018/02/raspbian-distro-lite.jpg)

![Selecci-n_084](/content/images/2018/02/Selecci-n_084.jpg)

![](/content/images/2018/02/upload-1518313856.jpg)

![](/content/images/2018/02/upload-1518313831.jpg)

Una vez hacemos el primer arranque y realizamos toda la primera configuración básica mediante *raspi-config*: teclado, idioma, **ip fija**, **activar SSH**, `sudo apt update`, `sudo apt upgrade` e instalar *byobu* (lo recomiendo para accesos SSH, realmente muy útil).

![Selecci-n_085](/content/images/2018/02/Selecci-n_085.jpg)

### Instalación de Nodejs + Node-RED

La instalación es tan sencilla, que únicamente tenemos que utilizar el **script *update-nodejs-and-nodered*** y él sólito va a instalar todo lo que necesitamos, y algo más:

`bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)`

![Selecci-n_086](/content/images/2018/02/Selecci-n_086.jpg)

Decimos que *YES* a todo, y el proceso comenzará a hacer cada una de las siguientes tareas:

![Selecci-n_087-1](/content/images/2018/02/Selecci-n_087-1.jpg)

*Hora de dedicarse a otra cosa,...tic,tac,...*

Al finalizar, tendremos algo como esto:  
![Selecci-n_089](/content/images/2018/02/Selecci-n_089.jpg)

Este script **nos ha instalado todo lo necesario!!!. Más fácil imposible!!!**

Vamos con el primer arranque!!!  
`node-red-start`

![Selecci-n_090](/content/images/2018/02/Selecci-n_090.jpg)

Y ahora ya podemos acceder vía web desde un navegador cualquiera poniendo la IP y el puerto por defecto que es el 1880:

![Selecci-n_091](/content/images/2018/02/Selecci-n_091.jpg)

**PIM-PAM!!!** ya tenemos nuestro servidor *Node-RED* funcionando!!!

### Mi primer Flow.

Cómo podeis ver *Node-RED* tiene bastantes nodos que podemos utilizar, además al instalarlo en una Raspberry Pi, nos ha instalado algunos nodos adicionales y específicos.

![node_red_raspi3](/content/images/2018/02/node_red_raspi3.png)

Pero os voy a explicar con un sencillo ejemplo utilizando el **node de entrada Twitter**, como funciona el concepto de *Node-RED*.  
Primero buscaremos el nodo de entrada de Twitter (In) y lo arrastramos a la zona de definición del flujo.

![nodered_twitter01](/content/images/2018/02/nodered_twitter01.png)

Ahora haciendo doble-click sobre el nodo se nos abrirá una zona donde poder configurar ese nodo en concreto.

![Selecci-n_094](/content/images/2018/02/Selecci-n_094.jpg)

El campo mas importante es la cuenta de Twitter. Deberemos autorizar a nuesra aplicación a utilizar el típico token de autorización. Es sencillo, solo tenemos que logarnos con nuestra cuenta de Twitter y darle al botón de *Autorizar la aplicación*.

![Selecci-n_095](/content/images/2018/02/Selecci-n_095.jpg)

Yo estoy utilizando una cuenta bot que tengo creada en Twitter para hacer las pruebas y que se llama *@qtrsite*. En este caso, vamos a determinar que las búsquedas sean para todos los tweets públicos y que lleven el hashtab #innovacion

![Selecci-n_096](/content/images/2018/02/Selecci-n_096.jpg)

Una vez definimos el nodo de entrada y para que nuestro sistema *Node-RED* se entere, tenemos que hacer un *Deploy*. Es importante, ya que de esa forma el flujo que estamos viendo será activado y comenzará a funcionar.

Pero antes vamos a añadir un segundo nodo: un nodo que recoja todos esos tweets que tiene el hashtag #innovacion y lo deje en un fichero de texto:

![Selecci-n_097](/content/images/2018/02/Selecci-n_097.jpg)

Ahora solo falta conectar ambos nodos y hacer el *Deploy* de nuestro primer flujo.

![Selecci-n_098](/content/images/2018/02/Selecci-n_098.jpg)

También podemos activar el modo *Debug*, simplemente seleccionando la pestaña que se encuentra en el menú de la derecha. Ahí podremos ver información de todos los nodos o simplemente de aquellos que nos interesa, utilizando un filtro.

Y si no, aquí tenemos el resultado (ojo, que hay límites en las búsquedas, por lo que si son muy abiertas, con muchos resultados, cómo la del ejemplo, causa problemas de rating superado)

![Selecci-n_101](/content/images/2018/02/Selecci-n_101.jpg)

Ahora toca seguir descubriendo esta fantástica herramienta para automatizar *casi todo.*
