---
layout: ../../layouts/post.astro
title: "Mejora la presentación de Firefox"
pubDate: 2007-05-29
description: "Gracias a esta entrada  (), descubro una de característica que desconocía de Firefox: modificar el contenido web que se muestra mediante CSS"
author: "akirasan"
isPinned: false
excerpt: "Gracias a esta entrada  (), descubro una de característica que desconocía de Firefox: modificar el contenido web que se muestra mediante CSS"
image:
  src: ""
  alt: "Mejora la presentación de Firefox"
tags: []
---

Gracias a esta entrada [Digg](http://www.digg.com/ "http://www.digg.com/") ([Web controls are ugly in Firefox!](http://www.digg.com/linux_unix/Web_controls_are_ugly_in_Firefox)), descubro una de característica que desconocía de Firefox: modificar el contenido web que se muestra mediante CSS. Los que habitualmente utilizamos Firefox para navegar, nos hemos dado cuenta que el tema de los botones, los checkbox, los campos de texto y los radiobuttons no son precisamente de lo último, vamos que aparecen al mas puro estilo *clásico de Windows*. Un claro ejemplo lo tenemos en la página de [Google](http://www.google.es/ "http://www.google.es/"), por ejemplo:

![botones_antes.jpg](http://www.akirasan.net/uploads/2007/05/botones_antes.jpg "botones_antes.jpg")

En [OS Novice](http://osnovice.blogspot.com/2007/05/firefox-controls-are-ugly.html "http://osnovice.blogspot.com/2007/05/firefox-controls-are-ugly.html"), existe una ampliación para el fichero forms.css que nos permite conseguir un acabo mas amigable a estos controles, pero para Linux (Ubuntu en este caso), "*el problema*" es que utiliza una hoja de estilos y procedimiento (adaptándolo al mundo Windows), no me ha funcionado.

Inicialmente traté de modificar este CSS para adaptarlo al Firefox 2.0 sobre Windows, pero me dí cuenta que tardaría mucho (he tenido que buscar documentación de Mozilla, ya que no es un CSS 100%). Total que comencé a buscar por la red, si alguien había hecho lo mismo pero para Windows. Yo, no he encontrado nada,...lo mejor que he podido encontrar es una modificación de [Philippe Wittenbergh para Mac y Firefox 1.9](http://emps.l-c-n.com/articles/94/widgets-for-firefox "http://emps.l-c-n.com/articles/94/widgets-for-firefox"), que en Windows y la versión 2.0 ha funcionado bastante bien.

Partiendo de esta versión ([prettywidgets-1.9](http://emps.l-c-n.com/file_download/5 "http://emps.l-c-n.com/file_download/5")) de Philippe W. he realizado un par de modificaciones:

* Adaptar el *forms.css* a la versión 2.0 de Firefox.
* Corregir los checkbox que no aparecían de forma correcta (vamos que no aparecían).
* Incluir también las listas (combobox)

Todo ello para conseguir un resultado como este:

![botones_despues.jpg](http://www.akirasan.net/uploads/2007/05/botones_despues.jpg "botones_despues.jpg")

Si quieres adaptar tu Firefox 2.0 de Windows con esta mejora solo tienes que seguir los siguientes pasos:

* **Bájate** el paquete completo (*forms.css* e imágenes) [de aquí](http://www.akirasan.net/firefox_style.zip "http://www.akirasan.net/firefox_style.zip onclick=").
* **Cierra** la sesión de Firefox (pero antes acaba de leer esto,...)
* Renombra el fichero ***forms.css*** a ***forms.css.old*** (por ejemplo) que encontraras normalmente en "*C:\Archivos de programa\Mozilla Firefox\res\*"
* Descomprime el .zip en **este mismo directorio**.
* **Arranca el Firefox** y a disfrutar de unos botones supermolones!!!
