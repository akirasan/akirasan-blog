---
layout: ../../layouts/post.astro
title: "IDE Arduino: Como habilitar acceso al puerto USB"
pubDate: 2015-04-02
description: "Si desde el IDE de Arduino tienes problemas con tu usuario para acceder al puerto USB donde está conectada tu placa Arduino, tal vez sea por"
author: "akirasan"
isPinned: false
excerpt: "Si desde el IDE de Arduino tienes problemas con tu usuario para acceder al puerto USB donde está conectada tu placa Arduino, tal vez sea por"
image:
  src: ""
  alt: "IDE Arduino: Como habilitar acceso al puerto USB"
tags: ["Linux", "Arduino"]
---

Si desde el IDE de Arduino tienes problemas con tu usuario para acceder al puerto USB donde está conectada tu placa Arduino, tal vez sea porque tu usuario no tenga los permisos adecuados.

![arduino ide usb](/content/images/2015/04/arduino_ide_usb.jpg)

Para solucionar el problema debemos darle los permisos a los grupos: **dialout** y **uucp**. El grupo **dialout** es necesario para los dispositivos que se conectan por */dev/ttyACMX* (Arduino UNO, por ejemplo). Y el grupo **uucp** es para los dispositivos que utilizan */dev/ttyUSBX* (normalmente por placas del estilo Arduino nano, mini,...).

Le podemos dar a nuestro usuario ambos permisos y listo!!!, así no nos preocupamos mas. Una forma sería:

```auto
sudo usermod -a -G dialout usuario
sudo usermod -a -G uucp usuario
```

Ahora necesitaremos dar acceso a nuestro usuario al fichero de bloqueos. Es un directorio se crea en tiempo de arranque según las especificaciones del fichero ***/usr/lib/tmpfiles.d/legacy.conf*** y únicamente se tiene acceso mediante *root*, por lo que deberemos modificarlo para especificar el grupo **lock** y luego dar permisos a nuestro usuario.

Podemos ver los permisos que suele tener:

```auto
ls -ld /run/lock
drwxrwxrwt 3 root root 80 abr  1 19:08 /run/lock
```

Ahora modificamos el fichero de configuración *legacy.conf* de la siguiente forma:

Siendo *root* copiamos a */etc/tmpfiles.d/* el fichero *legacy.conf* y editamos esa copia:

```auto
cp /usr/lib/tmpfiles.d/legacy.conf /etc/tmpfiles.d/
nano /etc/tmpfiles.d/legacy.conf
```

Cambiando la linea donde se define el usuario y grupo del directorio /run/lock:

```auto
#d /run/lock/lockdev 0775 root root -
d /run/lock/lockdev 0775 root lock -
```

Ahora le asignamos a nuestro usuario el grupo *lock*:

```auto
sudo usermod -a -G lock usuario
```

Si nos dice que no existe el grupo *lock*, lo podemos crear previamente así:

```auto
addgroup lock
```

Reiniciamos y si todo ha ido bien, deberíamos pasar de esto:

```auto
ls -ld /run/lock
drwxrwxrwt 3 root root 80 abr  1 19:08 /run/lock
```

a esto:

```auto
ls -ld /run/lock
drwxrwxrwt 3 root lock 80 abr  1 21:08 /run/lock
```
