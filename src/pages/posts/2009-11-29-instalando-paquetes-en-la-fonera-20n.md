---
layout: ../../layouts/post.astro
title: "Instalando paquetes en la Fonera 2.0n"
pubDate: 2009-11-29
description: "Si en la distribución de linux que viene en la Fonera 2.0n queremos instalar otras aplicaciones linux, podemos utilizar el repositorio de  ("
author: "akirasan"
isPinned: false
excerpt: "Si en la distribución de linux que viene en la Fonera 2.0n queremos instalar otras aplicaciones linux, podemos utilizar el repositorio de  ("
image:
  src: ""
  alt: "Instalando paquetes en la Fonera 2.0n"
tags: []
---

Si en la distribución de linux que viene en la Fonera 2.0n queremos instalar otras aplicaciones linux, podemos utilizar el repositorio de [Kamikaze](http://downloads.openwrt.org/kamikaze/8.09.2-RC2/brcm47xx/packages "http://downloads.openwrt.org/kamikaze/8.09.2-RC2/brcm47xx/packages") (versión de [OpenWrt](http://openwrt.org/ "http://openwrt.org/") que está compilado para distribuciones de este tipo). En este repositorio podemos encontrar el *amule*, *nano*, *htop*, etc,...varias aplicaciones muy interesantes que mejoran la distribución de la Fonera.

Antes de comenzar a instalar cosillas de este repositorio, debemos configurar el ***opkg*** para que nos permita instalar los paquetes en el disco USB (recordar que la Fonera no tiene mucho espacio y debemos utilizar un disco USB).

En este ejemplo, voy a instalar el ***nano*** en la Fonera, un editor de texto "*mejor*" que el sencillo ***vi***:

> Preparamos el opkg para que permita instalar el disco USB. Para ello editamos el fichero de configuración que se encuentra en ***/etc/opkg.conf*** y añadimos la siguiente linea:
>
> ```auto
> dest usb /tmp/mounts/Disc-A1
> ```
>
> Verifica el path, para que sea el disco/partición USB que quieres
>
> Luego ejecutamos el comando:
>
> ```auto
> opkg -d usb install http://downloads.openwrt.org/kamikaze/8.09.2-RC2/brcm47xx/packages/nano_2.0.7-1_mipsel.ipk
> ```
>
> Ahora ya tenemos instalado el ***nano***, pero si tratamos de ejecutarlo nos saldrá un error como este:
>
> ```auto
> Error opening terminal: xterm
> ```
>
> Esto es debido a que este programa (y otros como el htop) van a consultar la variable de entorno **TERMINFO**, la cual no existe. Hay que añadir la exportación de esta variable en el fichero ***profile*** que se encuentra en ***/jffs/etc/profile*** para corregir el error (verifica el path del terminfo):
>
> ```auto
> export TERMINFO=/tmp/run/mountd/sda1/usr/share/terminfo
> ```
>
> Si ahora ejecutas el ***nano***, debería funcionar!!!,...

  
Este tip también se puede utilizar en otras distribuciones de routers con linux con OpenWrt.
