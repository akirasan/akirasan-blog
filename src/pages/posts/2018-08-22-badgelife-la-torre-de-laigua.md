---
layout: ../../layouts/post.astro
title: "BadgeLife - La Torre de l'Aigua"
pubDate: 2018-08-22
description: "Lo de hacer PCB’s (Printed Circuit Board) engancha bastante."
author: "akirasan"
isPinned: false
excerpt: "Lo de hacer PCB’s (Printed Circuit Board) engancha bastante."
image:
  src: "/content/images/2018/08/IMG_20180814_155052-1.jpg"
  alt: "BadgeLife - La Torre de l'Aigua"
tags: ["PCB", "DIY", "maker", "KiCAD", "Ripolab"]
---

Lo de hacer PCB’s (Printed Circuit Board) engancha bastante.

Cuando desde [l’Ajuntament de Sabadell](http://web.sabadell.cat/) se pusieron en contacto con [Ripolab Hacklab](http://ripolab.org) para hacer alguna actividad en el evento gratuito y anual Saba-TiC para divulgar la innovación y nuevas tecnologías (ya va por su tercera edición), no me lo pensé dos veces y Ripolab tenía que estar presente de alguna forma.

Desde que conocí el mundo de la electrónica digital sobre el 2011 con Arduino (un mundo totalmente nuevo para mí), siempre he pensado lo importante que es hacer divulgación de este mundo que parece tan técnico, pero que puede ser muy creativo y divertido. Por ello he querido hacer este año un taller de soldadura electrónica familiar. La idea como siempre es la de hacer divulgación y acercar el mundo maker a toda la familia, y sobretodo a ver la tecnología como una herramienta cercana y creativa, y que no se vea como algo técnico, especialista y muy complejo que nadie puede entender a menos que seas un ingeniero.

Para que se vea que todo es un proceso creativo que querido hacer esta entrada en el blog, para ilustrar los pasos por los que ha pasado este proyecto, hasta que se ha convertido en una badge PCB, una especie de chapa-insignia hecha con electrónica.

#### La idea

En este caso tenía más o menos claro el diseño del circuito electrónico, algo sencillo, con pocos [componentes THT](https://es.wikipedia.org/wiki/Tecnolog%C3%ADa_de_agujeros_pasantes) para que sea sencillo soldar: un par de leds RGB, un microprocesador [ATtiny85](https://www.microchip.com/wwwproducts/en/ATtiny85) (que puedo programar fácilmente desde el IDE de Arduino), una pila, un interruptor y un pulsador para interactuar con la badge de alguna forma. Todo claro,…pero lo peor fue resolver la pregunta ¿Qué forma va a tener la badge?

La propuesta del taller nació de Ripolab, por lo que no había una especificación de diseño, forma o características del diseño de la badge. Así que tocó sacar el lado creativo y hacer algunos bocetos en papel.

![](/content/images/2018/08/IMG_20180722_122200.jpg)

![](/content/images/2018/08/IMG_20180723_000259.jpg)

Si, si, parece una cebolla, y es que Sabadell en su escudo lleva una cebolla, así que podría ser un buen símbolo reconocible. Pero finalmente me gustó más la Torre de l’Aigua.

#### La Torre de l’Aigua

[La Torre de l’Aigua](https://es.wikipedia.org/wiki/Torre_de_l%27Aigua_de_Sabadell) es un depósito de agua modernista considerado uno de los edificios más emblemáticos de la ciudad. Su construcción se inició en 1916 y acabó en 1918, por lo que justo este año se cumple su centenario (ideal para rendirle homenaje). Funcionó como depósito entre 1922 y 1967, y es considerado uno de los 100 elementos del Patrimonio Industrial de Cataluña.

![800px-Torre_de_l-aigua-sabadell](/content/images/2018/08/800px-Torre_de_l-aigua-sabadell.jpg)

#### Fabricación Digital. Diseño gráfico PCB

Ya tenemos la idea, el circuito que vamos a implementar. Ahora hay que pasar a la fabricación digital. Convertir esa idea en información digital para que se pueda construir.

Lo ideal es trabajar con un diseño vectorizado, yo para eso utilizo [Inkscape](https://inkscape.org/es/). Recopilé algunas fotos de la torre y las transformé utilizando la opción de Inkscape “Vectorizar mapa de bits”. Hice un mix entre varias y modifiqué alguna de las proporciones, porque la idea era tener una parte central donde cada niño/niña pudiera escribir su nombre en la badge.

![predise-o_torre_vectorizar](/content/images/2018/08/predise-o_torre_vectorizar.png)

Este proceso es algo lento y engorros cuando no conoces mucho la herramienta y a la hora de exportar y transformar los vectores para usarlos posteriormente, puede no ser un dolor de cabeza, ya que en ocasiones no se interpretan bien o las figuras no son polígonos cerrados por completo y eso causa errores.

![diseno_PCB_inkscape1](/content/images/2018/08/diseno_PCB_inkscape1.png)

![diseno_PCB_inkscape2](/content/images/2018/08/diseno_PCB_inkscape2.png)

#### Diseño electrónico de la PCB

Ahora que ya tenemos digitalmente el diseño gráfico, debemos utilizar otra herramienta para el diseño del circuito. Para ello utilizo [KiCAD](http://kicad-pcb.org/). Primero diseñamos es esquema lógico con los componentes y luego seleccionamos cuál será su representación real y por lo tanto su footprint.

![esquemaPCB](/content/images/2018/08/esquemaPCB.png)

Mezclamos el diseño gráfico con los componentes del circuito y ya tenemos una versión básica de una placa electrónica con una forma diferente.

![PCB_disenobasico1](/content/images/2018/08/PCB_disenobasico1.png)

![PCB_disenobasico1_render](/content/images/2018/08/PCB_disenobasico1_render.png)

Ahora añadimos el resto de elementos gráficos (trazos, límites, contornos,…) y logotipos como los del Ajuntament de Sabadell, Saba-TiC, RS-Components (quienes nos han facilitado parte del material para poder realizar el taller y construir la placa), Ripolab Hacklab…y ya tenemos algo chulo.

![PCB_disenofinal](/content/images/2018/08/PCB_disenofinal.png)

Una vez hemos revisado el circuito y todas las conexiones, **yo suelo imprimir en papel y a escala el diseño**. Esto me permite ver tanto las dimensiones reales y probar que los componentes van a encajar bien.

![](/content/images/2018/08/IMG_20180822_162741.jpg)

![](/content/images/2018/08/IMG_20180822_162751.jpg)

En este caso, y siendo para un taller familiar donde mucha gente será la primera vez que coge un soldador en su vida, he utilizado componentes THT **espaciados a 2,54mm** (si, los leds RGB van a ir un poco justos), a excepción del porta pilas CR2032, que es un componente de soldadura superficial, por lo tanto **he modificado el footprint original** para que tenga mas superficie y sea mas sencillo.

![footprint_adaptado](/content/images/2018/08/footprint_adaptado.png)

#### Fabricación física

Para la fabricación física de la PCB **he vuelto a utilizar el servicio de [PCBWay](https://www.pcbway.com/)**. Hay que saber que cuando envías algo a fabricar, primero es verificado a nivel de diseño, es decir, *verifican que sean capaces de crear y cortar el diseño que le has enviado*. En este caso me recomendaron cambiar el diseño, ya que había partes muy finas, inferiores a 1,6mm. Eso podía provocar rotura de la placa durante la fabricación. Así que modifiqué el diseño y todo OK.

![pcb_KO](/content/images/2018/08/pcb_KO.png)

Hay que modificar un poco el diseño final para cumplir con los requisitos de fabricación.

![PCB_OK1](/content/images/2018/08/PCB_OK1.png)

![PCB_OK2](/content/images/2018/08/PCB_OK2.png)

Gracias a [RS-Components](https://es.rs-online.com/web/) que ha patrocinado parte de los componentes de este proyecto, cuando llegaron las PCBs no pude esperar a montar una.

![](/content/images/2018/08/IMG_20180814_103950.jpg)

![](/content/images/2018/08/IMG_20180814_105240.jpg)

![](/content/images/2018/08/IMG_20180814_105258.jpg)

![](/content/images/2018/08/IMG_20180814_155052.jpg)

Grata sorpresa que todo funciona correctamente y estamos a tiempo de tenerlo todo preparado para el taller de soldadura familiar el próximo 8 de septiembre.

Os dejo un vídeo con una demo que cargué en el ATTiny85 para verificar que todo funcionaba bien.

Y otra cosa que olvidaba!!! El código para poder hacerla funcionar con estos efectos simples, lo encontráis en mi repositorio de [Github](https://github.com/akirasan/Placas-PCB/tree/master/Saba-TIC%202018)
