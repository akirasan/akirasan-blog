---
layout: ../../layouts/post.astro
title: "Proyecto GPS+D90+Arduino (Parte 4 y \"última\")"
pubDate: 2012-03-09
description: "En esta última parte únicamente se trata de pasar el prototipo a una placa Arduino mini pro, la cual permite reducir el tamaño y poderlo enc"
author: "akirasan"
isPinned: false
excerpt: "En esta última parte únicamente se trata de pasar el prototipo a una placa Arduino mini pro, la cual permite reducir el tamaño y poderlo enc"
image:
  src: ""
  alt: "Proyecto GPS+D90+Arduino (Parte 4 y \"última\")"
tags: []
---

En esta última parte únicamente se trata de pasar el prototipo a una placa Arduino mini pro, la cual permite reducir el tamaño y poderlo encapsular mejor:

[![](http://static.zooomr.com/images/10172642_a847ea8b2c.jpg "http://static.zooomr.com/images/10172642_a847ea8b2c.jpg")](http://static.zooomr.com/images/10172642_a847ea8b2c_b.jpg)

Aquí algunas fotos de las primeras pruebas de campo con el sistema "a la vista":

[![](http://static.zooomr.com/images/10172661_0e0abd94b9_m.jpg "http://static.zooomr.com/images/10172661_0e0abd94b9_m.jpg")](http://static.zooomr.com/images/10172661_0e0abd94b9_b.jpg) [![](http://static.zooomr.com/images/10172657_e93781512e_m.jpg "http://static.zooomr.com/images/10172657_e93781512e_m.jpg")](http://static.zooomr.com/images/10172657_e93781512e_b.jpg)

He reciclado el conector del disparador para que me haga de caja. Para ello hay que desmontarlo todo y con la "dremel" vaciar el contenido de plástico para poder hacer hueco.

[![](http://static.zooomr.com/images/10172648_73c18e7ebb_m.jpg "http://static.zooomr.com/images/10172648_73c18e7ebb_m.jpg")](http://static.zooomr.com/images/10172648_73c18e7ebb_b.jpg) [![](http://static.zooomr.com/images/10172644_3ba656a313_m.jpg "http://static.zooomr.com/images/10172644_3ba656a313_m.jpg")](http://static.zooomr.com/images/10172644_3ba656a313_b.jpg)

He tenido que sacrificar los tres led's con indicaciones (aunque no descarto ponerlos en algún momento mediante LED's SMD) y únicamente me he quedado con el indicador del estado del módulo bluetooth, que realmente "*me da información*".

[![](http://static.zooomr.com/images/10172651_304d362db2_m.jpg "http://static.zooomr.com/images/10172651_304d362db2_m.jpg")](http://static.zooomr.com/images/10172651_304d362db2_b.jpg)

Le he incorporado un pequeño pulsador para poder realizar el reset, en caso que se quede sin señal o cualquier otra cosa. Así no tengo que quitarlo y volverlo a conectar (ya que la alimentación de la cámara es constante). La idea es colocarle un pequeño interruptor que permita apagar/encender y no tener que desmontarlo.

Para fijar el conector he utilizado, por primera vez, Sugru. Que aunque es muy versátil  y fácil de utilizar,...tiene su "que". Las pruebas de campo fueron muy bien aquí os dejo alguna foto del cacharro y alguna de las fotos geoposicionadas directamente desde la cámara.

[![](http://static.zooomr.com/images/10172660_58476f81c8.jpg "http://static.zooomr.com/images/10172660_58476f81c8.jpg")](http://static.zooomr.com/images/10172660_58476f81c8_b.jpg)

[![](http://static.zooomr.com/images/10172659_e877f8e5f8.jpg "http://static.zooomr.com/images/10172659_e877f8e5f8.jpg")](http://static.zooomr.com/images/10172659_e877f8e5f8_b.jpg)

Fotos geoposicionadas en Panoramio:

[![](http://static.zooomr.com/images/10172656_884a61e65b_m.jpg "http://static.zooomr.com/images/10172656_884a61e65b_m.jpg")](http://www.panoramio.com/photo/68168977) [![](http://static.zooomr.com/images/10172654_9345046ebe_m.jpg "http://static.zooomr.com/images/10172654_9345046ebe_m.jpg")](http://www.panoramio.com/photo/68168969) [![](http://static.zooomr.com/images/10172653_628eef326a_m.jpg "http://static.zooomr.com/images/10172653_628eef326a_m.jpg")](http://www.panoramio.com/photo/68168964)

PD: Tal vez haga una revisión 2 del proyecto (que he bautizado como **qtrArduD90**) con GPS incorporado y batería propia.
