---
layout: ../../layouts/post.astro
title: "Phishing - un caso práctico y como detectarlo"
pubDate: 2015-02-13
description: "Hoy he recibido un correo de CaixaBank, pero no eran los señores de , ha sido un caso de **phishing** o suplantación de identidad, con el fi"
author: "akirasan"
isPinned: false
excerpt: "Hoy he recibido un correo de CaixaBank, pero no eran los señores de , ha sido un caso de **phishing** o suplantación de identidad, con el fi"
image:
  src: ""
  alt: "Phishing - un caso práctico y como detectarlo"
tags: ["phishing", "seguridad"]
---

Hoy he recibido un correo de CaixaBank, pero no eran los señores de [CaixaBank](https://portal.lacaixa.es/), ha sido un caso de **phishing** o suplantación de identidad, con el fin de conseguir realizar alguna acción fraudulenta, como conseguir mis números de tarjetas, mi usuario/contraseña u otra información bancaría.

El término **phishing** proviene de la palabra inglesa *"fishing"* (pesca), haciendo alusión al intento de hacer que los usuarios *"muerdan el anzuelo"*.

¿Cómo identificar y actuar ante un posible caso?. Lo primero que tenemos que tener claro, que NINGUNA entidad bancaría hace un correo de este estilo, en el que solicita que te conectes para verificar tu identidad. Aquí el correo que he recibido y como he identificado de forma muy sencilla que no era un correo *"legal"*:

![phishing caixabank correo](/content/images/2015/02/phishing_caixabank.jpg)

Lo primero que tenemos que mirar es el emisor del correo, pero no el nombre suplantado (**"[servicio@lacaixa.es](mailto:servicio@lacaixa.es)"**), sino la dirección del correo REAL **email.wpengine.com**

Luego el link al que nos hacen ir, **no es un dominio de La Caixa**, es decir, deberían enviarnos a una dirección del estilo: <https://portal.lacaixa.es/> o *< algo >*.**lacaixa.es**. Vemos que pone (cuando pasamos el cursor por encima, sin hacer click!!!) **loc1.lacaixa.es.lgpallets.it**, el dominio de esta dirección es **lgpallets.it**, por lo tanto **no es lacaixa.es !!!**

**¿Que hacer?**, pues muy sencillo, si usas **Gmail** como es mi caso, denunciarlo para que Google tome las medidas adecuadas y otros usuarios no sean objetivos de estos correos.

![phishing la caixa denunciar](/content/images/2015/02/phishing_caixabank_denunciar.jpg)

**Navega seguro!!!**,...además si no soy cliente de La Caixa,... :/
