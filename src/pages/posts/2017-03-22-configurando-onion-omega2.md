---
layout: ../../layouts/post.astro
title: "Configurando una Onion Omega2+"
pubDate: 2017-03-22
description: "La Omega2+ es un dispositivo IoT de , que tiene una pinta muy buena!!!"
author: "akirasan"
isPinned: false
excerpt: "La Omega2+ es un dispositivo IoT de , que tiene una pinta muy buena!!!"
image:
  src: "/content/images/2017/03/IMG_20170308_211055-1.jpg"
  alt: "Configurando una Onion Omega2+"
tags: ["IoT", "Linux", "onion"]
---

La Omega2+ es un dispositivo IoT de [Onion](https://onion.io/), que tiene una pinta muy buena!!!

Después de ver el [vídeo de Oscar](https://www.youtube.com/watch?v=DuYg8-YZ3E4) de [BricoGeek](http://tienda.bricogeek.com) y sabiendo que se habían convertido en distribuidores oficiales en Europa, me entraron unas ganas locas de probar esta pequeña placa, que apunta maneras en el mundo del IoT.

![](/content/images/2017/03/IMG_20170308_211055.jpg)

Las primeras cosas que me llamaron la atención cuando salió en campaña crowdfunding en [Kickstarter](https://www.kickstarter.com/projects/onion/omega2-5-iot-computer-with-wi-fi-powered-by-linux?lang=es) (ahora está en [Indiegogo](https://www.indiegogo.com/projects/omega2-5-linux-computer-with-wi-fi-made-for-iot#/) fue: pequeño, barato (la Omega2 Plus sale por unos 10€), inalámbrico (Wifi), memoria interna y **con un Linux** corriendo en su interior (a parte tiene un slot para poner una tarjeta microSD y poder ampliar la capacidad).

Para funcionar sin problemas a parte de la **Omega2 Plus** por 10€, me compré una **Omega Power Dock** por 15,5€, una breadboard necesaria para poder alimentar la Omega2 sin problemas (de múltiples formas) y además nos permitirá acceder fácilmente a los pins GPIO de la placa.

![](/content/images/2017/03/IMG_20170308_211246.jpg)

Lo primero que tenemos que hacer es conectar la Omega2+ a la Power Dock, para ello hay que fijarse un poquito en la orientación que tiene que tener, para ello hay un pequeño dibujo que nos sirve de guía.

![](/content/images/2017/03/IMG_20170308_211740.jpg)

Una vez alimentada (yo he optado por alimentar mediante microUSB), la Omega2+ genera un punto de acceso Wifi para poder configurarla. El punto de acceso wifi tiene la descripción “Omega-” mas los cuatro últimos valores de la MAC del dispositivo. Nos conectamos a ese punto de acceso con **la contraseña por defecto “12345678”**.

![](/content/images/2017/03/Selecci-n_010.png)

![](/content/images/2017/03/Selecci-n_011.png)

Ahora abrimos un navegador web y podemos acceder al asistente (setup wizard) de configuración de dos formas: colocando en la URL <http://omega-ABCD.local/> (donde **ABCD** vuelven a ser los cuatro últimos dígitos de la MAC) o utilizando la dirección IP <http://192.168.3.1>

![](/content/images/2017/03/IMG_20170310_163237_20170321161306614.jpg)  
![](/content/images/2017/03/Selecci-n_024.png)

Bien!!! Ya tenemos acceso y comenzamos el wizard para configurar nuestra placa Omega2+. Tenéis que saber dos cosas importantes:

1. ```auto
   El usuario/contraseña por defecto es: **root / onioneer**
   ```
2. ```auto
   Si o si, hay que configurarle una conexión Wifi a nuestra red
   ```
3. ```auto
   Se va a actualizar solicita si el firmware que lleva no es el último
   ```

El resto de pasos son opcionales, cómo darla de alta en un servicio Cloud o instalar consola (una app tipo terminal).

![](/content/images/2017/03/Selecci-n_015.png)

Un tema importante, es que si se actualiza, NO APAGUEIS ni desconectéis!!!....y sobretodo dejadla un ratito una vez ha realizado el boot completo.

Una vez ha arrancado, se conectará a nuestra Wifi y será como un dispositivo mas. Aquí el tema es,…¿si se ha conectado a mi wifi y tengo configurado un servido DHCP, como se la IP que tiene asignada a la que conectarme?. Bueno yo he utilizado la opción de acceder a mi router y asignar una IP fija a la MAC del dispositivo, así sabré que dirección IP tiene.

El acceso a la Omega2+ se puede mediante un entorno web muy bien cuidado y sencillo que permite realizar tareas de mantenimiento como instalación/desintalación de aplicaciones, actualizar firmware, factory reset, abrir una consola de línea de comandos, activar acceso SSH, etc,…

![](/content/images/2017/03/Selecci-n_020.png)

![](/content/images/2017/03/Selecci-n_021.png)

No voy a alargar mucho mas éste post con las especificaciones técnicas que podéis consultar en la web: <https://docs.onion.io/omega2-docs/omega2p.html>

Hasta aquí esta primera aproximación a esta solución IoT que de saque me ha gustado mucho. Ahora faltará conectarle algún que otro sensor por GPIO y hacer un poquito de programación.

Toda la información está actualizada en su web de documentación: <https://docs.onion.io/omega2-docs/>
