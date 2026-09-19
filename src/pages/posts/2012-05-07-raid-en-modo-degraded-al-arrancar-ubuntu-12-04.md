---
layout: ../../layouts/post.astro
title: "RAID en modo Degraded al arrancar (Ubuntu 12.04)"
pubDate: 2012-05-07
description: "Una de las cosas que me he encontrado tras actualizar mi NAS de Ubuntu 11.04 a 12.04 es un tema con el RAID1 que tenía montado entre dos dis"
author: "akirasan"
isPinned: false
excerpt: "Una de las cosas que me he encontrado tras actualizar mi NAS de Ubuntu 11.04 a 12.04 es un tema con el RAID1 que tenía montado entre dos dis"
image:
  src: ""
  alt: "RAID en modo Degraded al arrancar (Ubuntu 12.04)"
tags: []
---

Una de las cosas que me he encontrado tras actualizar mi NAS de Ubuntu 11.04 a 12.04 es un tema con el RAID1 que tenía montado entre dos discos. Al arrancar el sistema detecta que existe un RAID y que uno de los discos no está bien (cual realmente no hay problema) y pide confirmación de arrancar el raid en modo *Degraded*. Este mecanismo de alerta es por si el raid se tiene como boot y por lo tanto el sistema operativo no puede arrancar. Hay que decir que mi RAID1 es de datos y no de sistema operativo, por lo que en cierta forma esto me daría igual.

Resulta que existe un bug reportado a Ubuntu con este fallo: <https://bugs.launchpad.net/ubuntu/+source/linux/+bug/990913> (he de comentar que mi versión no es la 12.04 Server, pero es lo mismo :) )

Para solucionarlo de forma temporal hasta que esté corregido, he optado por poner el parámetro **BOOT\_DEGRADED=true** como parámetro de arranque. De esta forma ignorará este advertencia y arrancará sin pedir confirmación. Estos son los pasos a seguir:

```auto
sudo nano /etc/initramfs-tools/conf.d/mdadm
```

Este será el fichero que editaremos:

```auto
# mdadm boot_degraded configuration
#
# You can run 'dpkg-reconfigure mdadm' to modify the values in this file, if
# you want. You can also change the values here and changes will be preserved.
# Do note that only the values are preserved; the rest of the file is
# rewritten.
#
# BOOT_DEGRADED:
# Do you want to boot your system if a RAID providing your root filesystem
# becomes degraded?
#
# Running a system with a degraded RAID could result in permanent data loss
# if it suffers another hardware fault.
#
# However, you might answer "yes" if this system is a server, expected to
# tolerate hardware faults and boot unattended.

BOOT_DEGRADED=false
```

...al final ponemos el parámetro ***BOOT\_DEGRADED=false*** a "***true***". Guardamos y reiniciamos.

OJO!!! que previamente tenéis que comprobrar que realmente tras arrancar el raid funciona correctamente y que tenéis este bug!!!. Para ver que tenéis el raid en perfecto estado tenéis que ejecutar este comando:

```auto
root@qtrnas:~# mdadm --query --detail /dev/md0
/dev/md0:
        Version : 1.2
  Creation Time : Thu Sep  8 21:29:33 2011
     Raid Level : raid1
     Array Size : 488376184 (465.75 GiB 500.10 GB)
  Used Dev Size : 488376184 (465.75 GiB 500.10 GB)
   Raid Devices : 2
  Total Devices : 2
    Persistence : Superblock is persistent

    Update Time : Mon May  7 15:36:44 2012
          State : clean
 Active Devices : 2
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 0

           Name : qtrnas:0  (local to host qtrnas)
           UUID : cfff5808:181b19bc:154931a0:1f3ecfc4
         Events : 7441

    Number   Major   Minor   RaidDevice State
       0       8       33        0      active sync   /dev/sdc1
       2       8       17        1      active sync   /dev/sdb1
```
