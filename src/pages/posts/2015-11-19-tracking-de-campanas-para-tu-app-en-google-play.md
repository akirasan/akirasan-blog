---
layout: ../../layouts/post.astro
title: "Tracking de orígines para tu app en Google Play"
pubDate: 2015-11-19
description: "Google ha liberado recientemente (en agosto de 2015) una utilidad de tracking de campañas, de tal forma que nos . Esta funcionalidad la enco"
author: "akirasan"
isPinned: false
excerpt: "Google ha liberado recientemente (en agosto de 2015) una utilidad de tracking de campañas, de tal forma que nos . Esta funcionalidad la enco"
image:
  src: "/content/images/2015/11/google_developers.JPG"
  alt: "Tracking de orígines para tu app en Google Play"
tags: ["Google", "analytics"]
---

Google ha liberado recientemente (en agosto de 2015) una utilidad de tracking de campañas, de tal forma que nos [permita conocer el origen de las descargas de nuestra aplicación Android](https://support.google.com/googleplay/android-developer/answer/6263332#sources). Esta funcionalidad la encontramos en el menú de **Statistics** en **User Acquisition**:

![](/content/images/2015/11/acquisition.jpg#small)

Los orígenes de adquisición ahora disponemos de una nueva entrada llamada **Tracked channels (UTM)**, mediante la cual podemos hacer tracking de forma similar a uso de tags de Google Analytics:  
![](/content/images/2015/11/acquisition_channel.jpg)  
En el detalle de esa opción veremos las campañas/orígenes que hemos definidos en los links de descarga hacia la Google Play Store de nuestra aplicación Android. Por ejemplo, yo he creado una origen de descarga llamado *"web"*, es decir, de esta forma podré saber el impacto que tiene mi link puesto en una *"web"* propia, pero aquí podemos indicar cualquier nombre de nuestra campaña/origen:  
![](/content/images/2015/11/acquisition_channel_UTM.jpg)

Para generar los links a nuestra aplicación, la recomendación es utilizar el [Google Play URL Builder](https://developers.google.com/analytics/devguides/collection/android/v4/campaigns#google-play-url-builder), un generador de URL's que nos simplifica los campos que debemos poner. Evidentemente podemos no utilizarlo y hacerlo de forma manual  
![](/content/images/2015/11/URL_creator.JPG)
