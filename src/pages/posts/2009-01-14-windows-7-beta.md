---
layout: ../../layouts/post.astro
title: "Windows 7 beta"
pubDate: 2009-01-14
description: "Ya sabréis que la nueva versión \"*del Windows*\" (conocía como Windows 7) está disponible para descargar en su versión beta, desde el sitio o"
author: "akirasan"
isPinned: false
excerpt: "Ya sabréis que la nueva versión \"*del Windows*\" (conocía como Windows 7) está disponible para descargar en su versión beta, desde el sitio o"
image:
  src: ""
  alt: "Windows 7 beta"
tags: []
---

Ya sabréis que la nueva versión "*del Windows*" (conocía como Windows 7) está disponible para descargar en su versión beta, desde el sitio oficial de Microsoft. La verdad es que descargarse las dos versiones, la de 32 como 64 bits ha sido toda una odisea, porque ha tardado la ostia!!. Yo utilicé los enlaces que publicaron desde [Genbeta](http://www.genbeta.com/2009/01/09-descarga-windows-7-beta-si-puedes "http://www.genbeta.com/2009/01/09-descarga-windows-7-beta-si-puedes"), y como utilizo linux preferí ejecutar un **wget** en fondo que se los fuera bajando. Aquí os dejo el comando que he utilizado (con las URLs):

> ***wget -cvb -o win7\_32b.log -t 0*** *http://download.microsoft.com/download/6/3/3/633118BD-6C3D-45A4-B985-F0FDFFE1B021/EN/7000.0.081212-1400\_client\_en-us\_Ultimate-GB1CULFRE\_EN\_DVD.iso*
>
> ***wget -cvb -o win7\_64b.log -t 0** <http://download.microsoft.com/download/6/3/3/633118BD-6C3D-45A4-B985-F0FDFFE1B021/EN/7000.0.081212-1400_client_en-us_Ultimate-GB1CULXFRE_EN_DVD.iso>*

  
Básicamente utilizo los comandos de:  **-c** (continuar carga en caso de desconexión), **-v** (información en el log), **-b** (proceso batch/fondo), **-o** (para especificar un fichero de log) y **-t** (máximo de reintentos, con 0 (cero) es infinito).

Después de dos días con múltiples reintentos (por parte del proceso de wget) ha conseguido bajarse la versión de Windows 7 para 32bits. La cual he montado en una máquina virtual mediante VirtualBox.

[![windows7b_vm](http://static.zooomr.com/images/6709988_f15a8fc304.jpg)](http://www.zooomr.com/photos/akirasan/6709988/ "Photo Sharing")

...bueno,...no creo que migre a Windows,...me quedo con Ubuntu.

Link para download Windows 7 [32 bits](http://download.microsoft.com/download/6/3/3/633118BD-6C3D-45A4-B985-F0FDFFE1B021/EN/7000.0.081212-1400_client_en-us_Ultimate-GB1CULFRE_EN_DVD.iso "http://download.microsoft.com/download/6/3/3/633118BD-6C3D-45A4-B985-F0FDFFE1B021/EN/7000.0.081212-1400_client_en-us_Ultimate-GB1CULFRE_EN_DVD.iso")/[64bits](http://download.microsoft.com/download/6/3/3/633118BD-6C3D-45A4-B985-F0FDFFE1B021/EN/7000.0.081212-1400_client_en-us_Ultimate-GB1CULXFRE_EN_DVD.ISO "http://download.microsoft.com/download/6/3/3/633118BD-6C3D-45A4-B985-F0FDFFE1B021/EN/7000.0.081212-1400_client_en-us_Ultimate-GB1CULXFRE_EN_DVD.ISO")

[Numero de serie oficial Windows 7](http://www.microsoft.com/windows/windows-7/beta-download.aspx "http://www.microsoft.com/windows/windows-7/beta-download.aspx")
