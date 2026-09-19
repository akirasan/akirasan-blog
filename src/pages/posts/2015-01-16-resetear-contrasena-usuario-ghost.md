---
layout: ../../layouts/post.astro
title: "Resetear contraseña usuario Ghost"
pubDate: 2015-01-16
description: "¿Necesitas resetear o recuperar la constraseña de tu usuario en Ghost y no tienes montada la notificación por correo en caso de olvido de co"
author: "akirasan"
isPinned: false
excerpt: "¿Necesitas resetear o recuperar la constraseña de tu usuario en Ghost y no tienes montada la notificación por correo en caso de olvido de co"
image:
  src: ""
  alt: "Resetear contraseña usuario Ghost"
tags: ["Linux", "Ghost", "blog", "tools"]
---

¿Necesitas resetear o recuperar la constraseña de tu usuario en Ghost y no tienes montada la notificación por correo en caso de olvido de contraseña?. Esta es una forma de hacerlo, accediendo a base de datos SQLite DB de forma manual y actualizando el registro con una nueva password (solo funciona si tienes tu propio servidor Ghost ;) )

Necesitarás acceder al servidor donde tienes instalado Ghost mediente un terminal y ejecutar los siguientes comandos para poder modificar la BD.

Una vez en un terminal/consola tienes que acceder al path donde tengas instalado Ghost, y en ese raíz encontrarás este directorio: **/content/data/**. Desde ese directorio (que es donde reside la BD SQLite por defecto a menos que hayamos cambiado su hubicación) ejecutamos:

```auto
sqlite3 ghost.db
```

Ahora necesitamos generar una password temporal (o no, eso ya es cosa de cada uno) codificada hash para **BCrypt**. Para ello se puede utilizar el siguiente generador online: [bcrypthashgenerator](http://bcrypthashgenerator.apphb.com/). Por ejemplo para la palabra *"password"* se obtiene un hash como este: *$2a$10$BQToDNdBtBKCvnrTmMi5m.NK.7i6Qx7YASs.jTkE86I5zqxzE8klC*

Con el hash (ese churro hash que nos ha dado), volvemos al terminal y ejecutamos la siguiente sentencia SQL:

```auto
UPDATE users SET password='<<PASTE_HASH_AQUI>>' WHERE email = '<<EMAIL_DEL_USUARIO_A_RESETEARS>>';
```

Y ahora por último salimos de la consola SQL mediante:

```auto
.exit
```

Ahora ya puedes logarte vía web con tu usuario (recuerda que es la dirección de email) y la contraseña nueva que hemos actualizado mediante SQL.

Como medida de seguridad se aconseja, una vez has entrado, modificar la password.
