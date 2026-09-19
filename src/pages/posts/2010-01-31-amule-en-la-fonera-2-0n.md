---
layout: ../../layouts/post.astro
title: "Amule en la Fonera 2.0n"
pubDate: 2010-01-31
description: "Una de las cosas que le falta a la fonera 2.0n es un cliente emule. Para ello uno de los que se puede utilizar es el . Hasta ahora había uti"
author: "akirasan"
isPinned: false
excerpt: "Una de las cosas que le falta a la fonera 2.0n es un cliente emule. Para ello uno de los que se puede utilizar es el . Hasta ahora había uti"
image:
  src: ""
  alt: "Amule en la Fonera 2.0n"
tags: []
---

Una de las cosas que le falta a la fonera 2.0n es un cliente emule. Para ello uno de los que se puede utilizar es el [amule](http://www.amule.org/ "http://www.amule.org/"). Hasta ahora había utilizado la versión 2.1, que únicamente funcionaba mediante linea de comandos, es decir con el cliente *amulecmd*. Para mejorar esta situación es utilizar un cliente gráfico como *amule-gui*, pero para ello es necesario instalar la versión 2.2.6 del amule, ya que la anterior tenía un bug que impedía su utilización (aunque lo ideal sería tener el amuleweb para acceso vía web a la aplicación, pero todo se andará).

Bueno al lio, para instalar el amule versión 2.2.6 que es la única que funciona correctamente con *amule-gui*, lo que tenéis que hacer  es lo siguiente:

> Previamente hay que tener configurada la fonera para utilizar el instalador de paquetes ***opkg*** sobre un disco USB. [Aquí explico como](http://www.akirasan.net/?p=635 "http://www.akirasan.net/?p=635").
>
> Si tienes instalada una versión previa del amule, primer asegúrate que no hay ninguno corriendo y desinstala ese paquete:  
> `killall amuled  
> opkg -d usb remove amule`
>
> Ahora previamente instalamos unas de las dependencias que he me encontrado al instalar esta versión. Es el paquete *libcryptoxx:*  
> `opkg -d usb install http://flash.fonera.be/FON2303/packages/mipsel/libcryptoxx_5.6.0_mipsel.ipk`
>
> Una instalada la librería de *crypto*, instalamos el amule 2.2.6:  
> `opkg -d usb install http://flash.fonera.be/FON2303/packages/mipsel/amule_2.2.6-1_mipsel.ipk`
>
> A mi no me han salido mas dependencias, pero si surgen únicamente hay que buscar en el repositorio el paquete necesario e instalar.  
> **Primer problema:** Al tratar de ejecutar el amuled me sale un error:  
> `root@Fonera:/root# amuled --help  
> amuled: can't load library 'libwx_baseu_net-2.8.so.0'`  
> Hay que instalar esta librería (la verdad es que ya la tenía instalada pero la he actualizado de la 2.8.7-2 a la 2.8.10-1:  
> `opkg -d usb install http://flash.fonera.be/FON2303/packages/mipsel/libwxbase_2.8.10-1_mipsel.ipk`
>
> Ejecuto nuevamente el amuled y perfecto ya funciona!!!. Me reconoce el fichero de configuración y los anteriores ficheros que estaba compartiendo:  
> `root@Fonera:/root# amuled  
> amuled: OnInit - starting timer  
> Initialising aMuled 2.2.6 using v2.8.10  
> Checking if there is an instance already running...  
> No other instances are running.  
> ERROR: WARNING Warning! You are running aMule as root.  
> Doing so is not recommended for security reasons,  
> and you are advised to run aMule as an normal  
> user instead.`
>
> `--------------------------------------------------  
> Warning! You are running aMule as root.  
> Doing so is not recommended for security reasons,  
> and you are advised to run aMule as an normal  
> user instead.  
> --------------------------------------------------`
>
> `ERROR: Info --- This is the first time you run aMule 2.2.6 ---`
>
> `More information, support and new releases can found at our homepage,  
> at www.aMule.org, or in our IRC channel #aMule at irc.freenode.net.`
>
> `Feel free to report any bugs to http://forum.amule.org  
> ListenSocket: Ok.  
> Loading temp files from /root/disco/aMule/Temp.  
> Loading PartFile 4 of 4  
> All PartFiles Loaded.  
> No shareable files found in directory: /root/disco/aMule/Incoming`

  
Ahora una vez arrancado el server *amuled*, desde otro PC, arranco el *amule-gui* le indico la IP puerto 4712 donde está la fonera y tachan!!! conecta y me muestra las descargas que tengo en linea. Perfecto!!!.

Como podéis ver el repositorio utilizado es ***[http://flash.fonera.be/FON2303/packages/mipsel](http://flash.fonera.be/FON2303/packages/mipsel "http://flash.fonera.be/FON2303/packages/mipsel")*** que podéis consultar para ver que aplicaciones se pueden instalar en la fonera. Aunque podéis utilizar otros como el [http://downloads.openwrt.org/snapshots/trunk/brcm47xx/packages/](http://downloads.openwrt.org/snapshots/trunk/brcm47xx/packages/ "http://downloads.openwrt.org/snapshots/trunk/brcm47xx/packages/") siempre y cuando tengáis presente que el paquete está compilado para **mipsel** (*flash.fonera.be* está poco surtido). En este último podéis encontrar el ***nano***, el ***htop***, muy útiles,...

Siguientes pasos: tratar de hacer que el cliente web del amule funcione,...

**ACTUALIZADO (31/01/2010)**: Por cierto para abrir los puertos en la fonera hya que añadir estas dos lineas en el fichero /etc/firewall.user ([ya expliqué como crearlo y configurarlo](http://www.akirasan.net/?p=627 "http://www.akirasan.net/?p=627")):

`iptables -I input_wan -p tcp --dport 4662 -m state --state NEW -j ACCEPT  
iptables -I input_wan -p tcp --dport 4672 -m state --state NEW -j ACCEPT`
