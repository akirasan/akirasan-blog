---
layout: ../../layouts/post.astro
title: "Pasos para migrar de Wordpress a Ghost"
pubDate: 2014-10-06
description: "Estos han sido los pasos que he seguido y links de ayuda para migrar el blog de Wordpress a Ghost:"
author: "akirasan"
isPinned: false
excerpt: "Estos han sido los pasos que he seguido y links de ayuda para migrar el blog de Wordpress a Ghost:"
image:
  src: ""
  alt: "Pasos para migrar de Wordpress a Ghost"
tags: []
---

Estos han sido los pasos que he seguido y links de ayuda para migrar el blog de Wordpress a Ghost:

* Exportar los post de Wordpress para posteriormente importarlos en Ghost [Migrating WordPress to Ghost](http://ghostforbeginners.com/how-to-transfer-blog-posts-from-wordpress-to-ghost/)
* Instalación de Ghost [How to Install Ghost on a Raspberry Pi Running Raspbian](http://www.howtoinstallghost.com/how-to-install-ghost-on-a-raspberry-pi/). Aquí hay que armarse de paciencia, porque la compilación de Node.js es lento,...iros a tomar un café. Por cierto, yo al final he pueso Ghost en */var/www/ghost* por tener coherencia con otros contenidos web.
* Arrancar y parar Ghost como un servicio. En el apartado de Init Script de la guia [Installing Ghost & Getting Started](http://docs.ghost.org/installation/deploy/#init-script-)
* Configurar Apache como reverse proxy a la dirección interna de Ghost. Esto es muy sencillo y recomendable. [How To Proxy Ghost Through Apache – For Security and Multi Blog Setup](http://www.allaboutghost.com/how-to-proxy-ghost-through-apache-for-security-and-multi-blog-setup/)
