---
layout: ../../layouts/post.astro
title: "EncFS como encriptar/desencriptar información en linux"
pubDate: 2014-06-03
description: "Desde la reciente desaparición de  (de dudosas y extrañas formas), los que utilizamos métodos de encriptación de datos sensibles, nos hemos"
author: "akirasan"
isPinned: false
excerpt: "Desde la reciente desaparición de  (de dudosas y extrañas formas), los que utilizamos métodos de encriptación de datos sensibles, nos hemos"
image:
  src: ""
  alt: "EncFS como encriptar/desencriptar información en linux"
tags: []
---

Desde la reciente desaparición de [Truecrypt](http://truecrypt.sourceforge.net/ "http://truecrypt.sourceforge.net/") (de dudosas y extrañas formas), los que utilizamos métodos de encriptación de datos sensibles, nos hemos visto en la necesidad de buscar una salida alternativa. Particularmente he implementado [EncFS](http://www.arg0.net/encfs "http://www.arg0.net/encfs") (FUSE-based cryptographic filesystem [http://en.wikipedia.org/wiki/EncFS](http://en.wikipedia.org/wiki/EncFS "http://en.wikipedia.org/wiki/EncFS")), básicamente por estas razones:

1. Compatible con varias plataformas: Linux, Windows (mediante [encfs4win](http://members.ferrara.linux.it/freddy77/encfs.html  "http://members.ferrara.linux.it/freddy77/encfs.html ") y Android (con al app [Encdroid](https://play.google.com/store/apps/details?id=org.mrpdaemon.android.encdroid "https://play.google.com/store/apps/details?id=org.mrpdaemon.android.encdroid")) (básicamente lo que necesito).
2. Fácilmente implementable y usable.
3. Encriptación a nivel de fichero individual, lo que permite sincronizar repositorio encriptado con [BitTorrentSync](http://www.bittorrent.com/sync "http://www.bittorrent.com/sync") de forma mas óptima entre diferentes dispositivos (mi server Linux y mi smartphone).

Al lío, lo primero es conocer que EncFS utiliza por un lado un repositorio (directorio) encriptado y otro donde temporalmente desencriptar aparecen los ficheros desencriptados.

Instalación en Linux (en mi caso Ubuntu):

```auto
akirasan@ubuntu:~$ sudo apt-get install encfs
```

Creamos un directorio donde encriptaremos la información y otro donde quedará visible (lo creo dentro de mi home de usuario, por ejemplo):

```auto
akirasan@ubuntu:~$ mkdir /home/akirasan/Encriptado
akirasan@ubuntu:~$ mkdir /home/akirasan/Desencriptado
```

Con el siguiente comando lo que hacemos es crear y definir el directorio como encriptado por EncFS:

```auto
akirasan@ubuntu:~$ encfs /home/akirasan/Encriptado /home/akirasan/Desencriptado
```

La primera vez que ejecutemos este comando nos solicitará el modo, para simplificar utilizamos el modo p - paranoid, que nos preguntará el password que le queremos asignar a la encriptación de los datos:

```auto
Creando nuevo volumen cifrado.
Por favor, elige una de las siguientes opciones:
pulsa "x" para modo experto de configuracion,
pulsa "p" para modo paranoia pre-configurado,
cualquier otra, o una linea vacia elegira el modo estandar.
?> p

Seleccionada configuración Paranoica.

Configuración finalizada. El sistema de ficheros a ser creado tiene
las siguientes propiedades:
Cifrado del sistema de ficheros: "ssl/aes", versión 3:0:2
Codificacion del nombre de fichero: "nameio/block", versión 3:0:1
Tamaño de la llave: 256 bytes
Tamaño de Bloque: 1024 bytes, incluyendo 8 bytes de la cabecera MAC
Cada fichero contiene una cabecera de 8 bytes con datos IV únicos.
Nombres de fichero encodeados usando el modo IV de encadenamiento.
El IV de los datos del archivo está encadenado al IV del nombre del archivo.
Agujeros en archivos pasados a través del ciphertext.

----------------------- ADVERTENCIA --------------------------
Se ha habilitado la opción de encadenamiento de vectores externos de inicialización.
Esta opción impide el uso de enlaces duros en el sistema de archivos. Sin enlaces duros, algunos programas pueden fall
Para más información, por favor revise la lista de correo de encfs.
Si desea elegir otra configuración, por favor pulse CTRL-C para abortar la ejecución y comience de nuevo.

Ahora tendrás que introducir una contraseña para tu sistema de ficheros.
Necesitaras recordar esta contraseña, dado que no hay absolutamente
ningún mecanismo de recuperación. Sin embargo, la contraseña puede ser cambiada
más tarde usando encfsctl.

Nueva contraseña Encfs:
Verifique la contraseña Encfs:
```

Una vez tenemos esto, quedará montada la unidad en formato encfs la carpeta desencriptada. Es ahí donde operaremos con normalidad.

```auto
akirasan@ubuntu:~$ df
S.ficheros 1K-blocks Usados Disponibles Uso% Montado en
/dev/sdc1  56547644 10293440 43358640 20% /
none              4 0 4 0% /sys/fs/cgroup
udev         494744 12 494732 1% /dev
tmpfs        100900 1084 99816 2% /run
none           5120 4 5116 1% /run/lock
none         504480 168 504312 1% /run/shm
none         102400 40 102360 1% /run/user
encfs      56547644 10293440 43358640 20% /home/akirasan/Desencriptado
```

Carpeta desencriptada con un fichero de prueba *test\_encriptado.txt*

```auto
akirasan@ubuntu:~/Desencriptado$ ll
total 12
drwxrwxr-x 2 akirasan akirasan 4096 jun 3 11:58 ./
drwxr-xr-x 30 akirasan akirasan 4096 jun 3 11:55 ../
-rw-rw-r-- 1 akirasan akirasan 6 jun 3 11:58 test_encriptado.txt
```

Carpeta encriptada con el fichero de prueba *test\_encriptado.txt -->*  
 *vwqtmiP3LTr,J,4GBDr4I1JH7jwrf6ua2UFZvSeX6C55o0*

```auto
akirasan@ubuntu:~/Encriptado$ ll
total 16
drwxrwxr-x 2 akirasan akirasan 4096 jun 3 11:58 ./
drwxr-xr-x 30 akirasan akirasan 4096 jun 3 11:55 ../
-rw-rw-r-- 1 akirasan akirasan 1091 jun 3 11:55 .encfs6.xml
-rw-rw-r-- 1 akirasan akirasan 22 jun 3 11:58 vwqtmiP3LTr,J,4GBDr4I1JH7jwrf6ua2UFZvSeX6C55o0
```

Una vez queramos cerrar la carpeta desencriptada hay que desmontarla con el siguiente comando:

```auto
akirasan@ubuntu:~$ fusermount -u /home/akirasan/Desencriptado
```

Y para montar nuevamente el mismo comando que antes cuando creamos inicialmente el directorio encriptado, el cual nos pedirá el password:

```auto
akirasan@ubuntu:~$ encfs /home/akirasan/Encriptado /home/akirasan/Desencriptado
```

A tener en cuenta dos cosillas de la carpeta encriptada:

* No copiar directamente ningún fichero sobre es carpeta, porque quedará destrotegido de la encriptación. Yo es la que sincronizo con BitTorrentSync y los ficheros de sincronización son visibles, sin problema.
* No borrar el fichero de control *.encfs.xml*
