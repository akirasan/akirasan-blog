---
layout: ../../layouts/post.astro
title: "Problema con Google Chrome \"No se pueden leer tus preferencias\""
pubDate: 2010-06-22
description: "No se porque extraña razón, Google Chrome en mi Ubuntu 10.04 ha comenzado a dar un error un poco extraño nada mas arrancarlo. Me aparece un"
author: "akirasan"
isPinned: false
excerpt: "No se porque extraña razón, Google Chrome en mi Ubuntu 10.04 ha comenzado a dar un error un poco extraño nada mas arrancarlo. Me aparece un"
image:
  src: ""
  alt: "Problema con Google Chrome \"No se pueden leer tus preferencias\""
tags: []
---

No se porque extraña razón, Google Chrome en mi Ubuntu 10.04 ha comenzado a dar un error un poco extraño nada mas arrancarlo. Me aparece un pop-up con el mensaje: "*No se pueden leer tus preferencias*.". Buscando, buscando,...no he encontrado ninguna solución, así que he tenido que buscarme las castañas en un 5 minutos ya lo tenía solucionado :P

El tema es que por alguna razón hay un par de ficheros en la configuración personal de mi usuario que se ha guardado con permisos para el root (tanto el usuario como el grupo root son los únicos que tienen permisos de lectura/escritura, de ahí el mensaje de no poder leer las preferencias). Para solucionarlo hay que seguir estos sencillos pasos:

> 1. Acceder al directorio donde se guarda la configuración personal:
>
> ```auto
> $ cd /home/<usuario>/.config/google-chrome
> ```
>
> 2. Cambiar propietario y grupo del fichero "Local State" (tiene que tener el root):
>
> ```auto
> $ sudo chown <usuario>:<grupo_usuario> Local\ State
> ```
>
> 3. Cambiar al directorio Default:
>
> ```auto
> $ cd /home/<usuario>/.config/google-chrome/Default
> ```
>
> 4. Hacer el mismo cambio pero en el fichero "Preferences" (también tiene que tener el root):
>
> ```auto
> $ sudo chown <usuario>:<grupo_usuario> Preferences
> ```

Normalmente el <usuario> y <grupo\_usuario> es el mismo.

Espero que os funcione.
