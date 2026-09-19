---
layout: ../../layouts/post.astro
title: "Cambiar hostname en Ubuntu"
pubDate: 2017-09-25
description: "*Ésto no es mas que una de esas entradas tipo \"apuntes mentales\" para consultar en el futuro.*"
author: "akirasan"
isPinned: false
excerpt: "*Ésto no es mas que una de esas entradas tipo \"apuntes mentales\" para consultar en el futuro.*"
image:
  src: "/content/images/2017/09/commandline.jpg"
  alt: "Cambiar hostname en Ubuntu"
tags: ["Linux", "ubuntu", "tools", "commandline"]
---

*Ésto no es mas que una de esas entradas tipo "apuntes mentales" para consultar en el futuro.*  
Para cambiar el hostname de nuestra distribución Ubuntu podemos utilizar el siguiente commandline:

> sudo hostnamectl set-hostname "nuevo\_nombre\_hostname"

Y si lo que queremos es saber el *hostname* que tiene:

> hostnamectl status
