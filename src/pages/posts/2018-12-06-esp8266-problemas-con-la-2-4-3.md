---
layout: ../../layouts/post.astro
title: "ESP8266 problemas con la 2.4.2"
pubDate: 2018-12-06
description: "Si has llegado aquí es porque tienes algún problema raro con el ESP8266. Sobretodo si estás utilizando el IDE de Arduino para programarlo."
author: "akirasan"
isPinned: false
excerpt: "Si has llegado aquí es porque tienes algún problema raro con el ESP8266. Sobretodo si estás utilizando el IDE de Arduino para programarlo."
image:
  src: "/content/images/2018/12/photo_2018-12-06_20-11-25.jpg"
  alt: "ESP8266 problemas con la 2.4.2"
tags: ["ESP8266", "IoT"]
---

Si has llegado aquí es porque tienes algún problema raro con el ESP8266. Sobretodo si estás utilizando el IDE de Arduino para programarlo.

Lo cierto es que mi ESP8266-01 llevaba unos tres años funcionando perfectamente para encender y apagar la calefacción de forma remota, accionando un relé conectado a la caldera. El funcionamiento era muy sencillo, pero una base para iniciarse en con éste módulo. El artículo que escribí en su día lo podéis consultar aquí: <http://akirasan.net/programar-modulo-wifi-esp8266-con-el-ide-de-arduino/>

![](/content/images/2018/12/photo_2018-12-06_20-04-43_2.jpg)

En fin, que quise actualizar la interface que devuelve el servidor web que genera el ESP8266, simplemente añadiendo un poco de color y un par de botones que enlazan con los servicios de encendido y apagado que tiene programado el servidor web. Para ello incluí en el HTML que se devuelve un poquito de [Bootstrap](https://getbootstrap.com/), para hacer rápido y sencillo.

Compilación perfecta, sin problemas,…pero aquí empezó mi dolor de cabeza durante un par de días. Cuando intentaba acceder por la IP interna que tenía el módulo, estos eran los resultados dispares:

* Desde Chrome móvil/tablet Android, daba **ERR\_ADDRESS\_UNREACHABLE** (no ve el dispositivo?? :( )
* Desde un terminal con CURL, me contestaba perfectamente con el código HTML.
* Desde Chrome desde un PC, HTML perfecto con Bootstrap.
* Desde Safari con un móvil iOS, funcionaba.
* Al cabo de un rato funcionando, si intentaba acceder no contestaba. La IP no respondía.

![](/content/images/2018/12/Screenshot_20181205-170927.jpg)

En fin, un galimatías de pruebas y situación que no entendía. Incluso volví a dejar el código inicial y nada, seguía sin funcionar.

### El secreto

Buscando una solución por foros, issues, páginas,…encontré algunos de los síntomas que tenía, incluso con ESP8266-12, pero ninguno con una solución clara y directamente aplicable. Pero encontré en un par de respuestas a los problemas de acceso que se realizara un **downgrade a la versión 2.3.0 del ESP8266** en el gestor de tarjetas (IDE Arduino).

![](/content/images/2018/12/Selecci-n_493.png)

Y efectivamente, aquel cambio corrigió todos los problemas. No se que tendrá la versión 2.4.2 del ESP8266, pero está claro que algo raro le pasa. Por ahora seguiré con la versión anterior.
