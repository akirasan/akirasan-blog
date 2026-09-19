---
layout: ../../layouts/post.astro
title: "Proyecto GPS+D90+Arduino (Parte 2)"
pubDate: 2012-01-19
description: "Comenzamos la segunda parte del proyecto con \"*un pequeño contratiempo*\". Ya me ha llegado el módulo Bluetooth para conectarlo con el Arduin"
author: "akirasan"
isPinned: false
excerpt: "Comenzamos la segunda parte del proyecto con \"*un pequeño contratiempo*\". Ya me ha llegado el módulo Bluetooth para conectarlo con el Arduin"
image:
  src: ""
  alt: "Proyecto GPS+D90+Arduino (Parte 2)"
tags: []
---

Comenzamos la segunda parte del proyecto con "*un pequeño contratiempo*". Ya me ha llegado el módulo Bluetooth para conectarlo con el Arduino. Se trata de un BT BC41713 de CSR ([aquí podréis ver las especificaciones](http://air.imag.fr/mediawiki/index.php/Wireless_Bluetooth_RS232_TTL_Transceiver_Module "http://air.imag.fr/mediawiki/index.php/Wireless_Bluetooth_RS232_TTL_Transceiver_Module")). Me ha salido por unos 7€ en ebay. Esto bluetooth viene configurado inicialmente como slave, lo que quiere decir que cualquier otro dispositivo (master) se puede conectar utilizando el pin que tiene por defecto, "1234". Como mi idea es que este módulo se conecte al GPS (que es slave) tengo que "*convertirlo*" en master. Para ello hay que poner a HIGH el pin34 (llamado POI11 o en mi caso/módulo KEY) y mediante comandos AT (*AT+ROLE=1*) ponerlo como *master*.

[![](http://static.zooomr.com/images/10153089_b180f48ff5_m.jpg)](http://static.zooomr.com/images/10153089_5479947d64_o.jpg)

...bueno esto era lo que yo pensaba, hasta que lo he probado y he visto que mi módulo no aceptaba este comando, ni otros comandos AT. *Arrebuscando* información por internet me he dado cuenta de **un inconveniente**: el *chipi-chipi* viene con un firmware llamado **HC-06** o **Linvor V1.5**, el cual acepta muy pocos comandos AT. Básicamente estos:

|  |  |  |
| --- | --- | --- |
| **Comando** | **Respuesta** | **Nota** |
| AT | OK | Conexión lista!!! |
| AT+VERSION | Linvor1.5 | Versión del firmware (firmware maldito!!!) |
| AT+BAUD**x** | OKyyyy | Configurar velocidad de datos donde **x** puede ser uno de esto números:   * 1 para 1200 bps * 2     2400 bps * 3     4800 bps * 4     9600 bps * 5    19200 bps * 6    38400 bps * 7    57600 bps * 8   115200 bps * 9   230400 bps * A   460800 bps * B   921600 bps * C  1382400 bps |
| AT+NAME**String** | OKsetname | Cambiar el nombre emitido por bluetooth |
| AT+PIN**xxxx** | OKsetpin | Pincode, por defecto 1234 |

...y lo mas grave **no permite el modo master** :(. [En este post tenéis info sobre el HC-06 o Linvor V1.5](http://byron76.blogspot.com/2011/09/one-board-several-firmwares.html "http://byron76.blogspot.com/2011/09/one-board-several-firmwares.html") (este hardware puede aceptar varios firmware, incluso existe un IDE para poder desarrollarlos uno mismo,...).

Llegados a este punto, solo me quedaba dos opciones: comprar un nuevo módulo *master* en ebay (10$) o tostar un firmware nuevo sobre el cacharro. Así que siguiendo el siguiente esquema, descargando el software [BlueSuiteCasira 1.24](http://hotfile.com/dl/99452413/13c201b/BlueSuiteCasira124.zip.html) y recuperando un viejo PC con puerto paralelo LPT y WindowsXP,...he actualizado el firmware!!!,...

[![](http://static.zooomr.com/images/10153090_92d63e008b.jpg "esquema")](http://static.zooomr.com/images/10153090_92d63e008b_b.jpg)

Evidentemente, y para un único uso, no he hecho ni una sola soldadura (bueno las del puerto LPT reaprovechado que he encontrado por casa). Así es como me ha quedado el "*engendro*" temporal. Lo increíble es que ha funcionado "*a la primera*":

[![](http://static.zooomr.com/images/10152964_6a4c2259fe_m.jpg)](http://static.zooomr.com/images/10152964_6a4c2259fe_b.jpg) [![](http://static.zooomr.com/images/10152963_93ab4f0e3c_m.jpg)](http://static.zooomr.com/images/10152963_93ab4f0e3c_b.jpg) [![](http://static.zooomr.com/images/10152965_32e6ce206c_m.jpg)](http://static.zooomr.com/images/10152965_32e6ce206c_b.jpg) [![](http://static.zooomr.com/images/10152966_baed86bbff_m.jpg)](http://static.zooomr.com/images/10152966_baed86bbff_b.jpg) [![](http://static.zooomr.com/images/10152967_49fc1f2725_m.jpg)](http://static.zooomr.com/images/10152967_49fc1f2725_b.jpg) 

Ahora ya tengo el módulo tal y como yo quería. Siguiente paso,...programar el Arduino con los componentes pinchados!!!

Si queréis mas información aquí os dejo los links que he utilizado yo. Tendréis mas detalle (paso a paso de la aplicación para *flashear*, aunque no tiene mucho secreto) y un poco mas de teoría:

[http://byron76.blogspot.com/](http://byron76.blogspot.com/ "http://byron76.blogspot.com/")

<http://curiosidadesford.blogspot.com/2011/01/modulo-bluetooth-bluetooth-module.html>

<http://www.neoteo.com/modulo-bluetooth-hc-06-android>
