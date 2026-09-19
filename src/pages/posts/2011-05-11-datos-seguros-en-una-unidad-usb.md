---
layout: ../../layouts/post.astro
title: "Datos seguros en una unidad USB"
pubDate: 2011-05-11
description: "LLevo unos días con la paranoia de tener una lugar físico (pendrive) y encriptado con mis datos mas personales. Así que he estado buscando y"
author: "akirasan"
isPinned: false
excerpt: "LLevo unos días con la paranoia de tener una lugar físico (pendrive) y encriptado con mis datos mas personales. Así que he estado buscando y"
image:
  src: ""
  alt: "Datos seguros en una unidad USB"
tags: []
---

LLevo unos días con la paranoia de tener una lugar físico (pendrive) y encriptado con mis datos mas personales. Así que he estado buscando y probando diferentes alternativas y workflow's sencillos y operativos tanto en Windows como en Linux.

**Almacenamiento de claves**

Al final me quedo con la solucción [KeePass](http://keepass.info/ "http://keepass.info/") (ojo no [KeePassX](http://www.keepassx.org/ "http://www.keepassx.org/") que aún no tiene soporte para *KeePass 2* (.kdbx*)*), que aunque tiene poco rodaje apuesto en que será a medio plazo una solución adoptada como "*standard*". Tiene cliente Windows y Linux, es sencillo, no necesito mucho mas y es portable tanto para Windows como para Linux (funciona con [mono 2.6 que habrá que instalar](http://sourceforge.net/projects/keepass/forums/forum/329220/topic/4503818 "http://sourceforge.net/projects/keepass/forums/forum/329220/topic/4503818")). El inconveniente por ahora es que la autoescritura con autodetección del sitio web en Linux no está muy elaborada (aunque si funciona la autoescritura). Solo espero que evolucione con el tiempo o que KeePassX agrege el soporte para KeePass 2.

**Almacenamiento de datos encriptados**

He mirado [TrueCrypt](http://www.truecrypt.org/ "http://www.truecrypt.org/") (unidad completa encriptada y contenedor encriptado) y [Free OTFE](http://www.freeotfe.org/ "http://www.freeotfe.org/")(windows)+[dm-crypt](http://www.saout.de/misc/dm-crypt/ "http://www.saout.de/misc/dm-crypt/")(linux). Aquí es donde tengo mas dudas, así que seguiré probando un poco mas. Los escenarios son estos:

* **TrueCrypt**
  + Unidad USB 4Gb: cliente portable de TrueCrypt para Windows + *fichero encriptado*
  + Cliente TrueCrypt instalado en Linux para leer el fichero contenedor del USB.
* **dm-crypt + Free OTFE**
  + Unidad USB 4Gb: partición total del USB encriptada con dm-crypt (la creación de esta partición es MUY sencilla desde la utilizada de Gestión de Discos que viene por defecto en Ubuntu, ojo!!! hay que instalar el ***cryptsetup*** (*sudo apt-get install cryptsetup*).
  + La comodidad en Linux es muy buena, nada mas introducir el stick USB detecta un partición como encriptada y te solicita password, la monta como unidad y lo podemos utilizar como un USB mas. En Windows hay que tener un cliente Free OTFE que puede ser portable (en otro USB a parte) o instalado. Se indica la partición y listo.

Lo chungo de todo esto, es que tengo
