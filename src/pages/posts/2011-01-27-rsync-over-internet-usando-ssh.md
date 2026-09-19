---
layout: ../../layouts/post.astro
title: "rsync over internet usando ssh"
pubDate: 2011-01-27
description: "Para sincronizar ficheros mediante ** a través de ** por internet, es tan sencillo como ejecutar el siguiente comando:"
author: "akirasan"
isPinned: false
excerpt: "Para sincronizar ficheros mediante ** a través de ** por internet, es tan sencillo como ejecutar el siguiente comando:"
image:
  src: ""
  alt: "rsync over internet usando ssh"
tags: []
---

Para sincronizar ficheros mediante *[rsync](http://es.wikipedia.org/wiki/Rsync "http://es.wikipedia.org/wiki/Rsync")* a través de *[ssh](http://es.wikipedia.org/wiki/Ssh "http://es.wikipedia.org/wiki/Ssh")* por internet, es tan sencillo como ejecutar el siguiente comando:

```auto
rsync -avz -e “ssh –p <puertoNAT>” usuario_remoto@host_remoto:/path_remoto/dir /path_local/dir/
```

Si no utilizas un puerto NAT configurado en el router de entrada y es el standard, osea el puerto 22 te puedes ahorrar el parámetro "*-p <puertoNAT>*"
