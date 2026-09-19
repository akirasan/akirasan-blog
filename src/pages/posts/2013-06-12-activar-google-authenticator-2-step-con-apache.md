---
layout: ../../layouts/post.astro
title: "Activar Google Authenticator 2-Step con Apache"
pubDate: 2013-06-12
description: "Si quieres activar la verificación en dos pasos de Google sobre Apache, puedes hacerlo siguiendo estos pasos que detallo a continuación. En"
author: "akirasan"
isPinned: false
excerpt: "Si quieres activar la verificación en dos pasos de Google sobre Apache, puedes hacerlo siguiendo estos pasos que detallo a continuación. En"
image:
  src: ""
  alt: "Activar Google Authenticator 2-Step con Apache"
tags: []
---

Si quieres activar la verificación en dos pasos de Google sobre Apache, puedes hacerlo siguiendo estos pasos que detallo a continuación. En este caso mi escenario era habilitar a un site nuevo (https por puerto 81) que se accederá desde fuera de la red (internet) por un NAT definido en el router, dejando los puertos estandars 80 y 443 para acceso interno al mismo site y por lo tanto sin método de autenticación en dos pasos.

Lo primer que nos encontramos es un requerimiento de Apache2. Se trata de tener la utilidad apxs2 (<http://httpd.apache.org/docs/2.2/programs/apxs.html>), necesaria para construir e instalar módulos de Apache. Por lo tanto es necesario instalar el paquete ***apache2-prefork-dev***:

```auto
sudo apt-get install apache2-prefork-dev
```

Luego bajamos el código fuente del módulo y lo compilamos para nuestra distribución. Es importante tener de las herramientas de compilación y fuentes del kernel instaladas, si no las tenéis buscar info por internet, es sencillo y no es el objetivo de este tutorial:

```auto
wget https://google-authenticator-apache-module.googlecode.com/files/GoogleAuthApache.src.r10.bz2
```

Ahora descomprimimos el módulo con ***tar jxvf GoogleAuthApache.src.r10.bz2**:*

```auto
root@qtrnas:~/# tar jxvf GoogleAuthApache.src.r10.bz2

google-authenticator-apache-module/
google-authenticator-apache-module/base32.h
google-authenticator-apache-module/sha1.c
google-authenticator-apache-module/mod_authn_google.c
google-authenticator-apache-module/login.cgi
google-authenticator-apache-module/sha1.h
google-authenticator-apache-module/hmac.h
google-authenticator-apache-module/googleauth.conf
google-authenticator-apache-module/base32.c
google-authenticator-apache-module/hmac.c
google-authenticator-apache-module/README
google-authenticator-apache-module/Makefile
```

Verificamos/modificaremos que en el fichero ***Makefile*** (de donde hemos descomprimido) existe la entrada de *APXS*, y verificamos que los paths de instalación del módulo es correcto, ya que utilizaremos Apache2 (buscar la opción *install* dentro del fichero *Makefile*):

```auto
sudo nano Makefile
```

```auto
APXS=apxs2
```

Cambiar esta linea...:

```auto
install: all
         sudo cp .libs/mod_authn_google.so /etc/httpd/modules/
```

...por esta otra

```auto
install: all
         sudo cp .libs/mod_authn_google.so  /usr/lib/apache2/modules/
```

Lanzamos el ***make*** y el ***make install*** si todo ha ido bien con el make:

```auto
root@qtrnas:~/google-authenticator-apache-module# make

apxs2 -c mod_authn_google.c base32.c hmac.c sha1.c
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
mod_authn_google.c: In function 'ga_check_password':
mod_authn_google.c:310:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 7 has type 'apr_time_t' [-Wformat]
mod_authn_google.c: In function 'ga_get_realm_hash':
mod_authn_google.c:383:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 7 has type 'apr_time_t' [-Wformat]
mod_authn_google.c:404:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 8 has type 'apr_time_t' [-Wformat]
mod_authn_google.c: In function 'do_cookie_auth':
mod_authn_google.c:421:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 7 has type 'apr_time_t' [-Wformat]
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
/usr/share/apr-1.0/build/libtool --silent --mode=link --tag=disable-static i686-linux-gnu-gcc -o mod_authn_google.la  -rpath /usr/lib/apache2/modules -module -avoid-version  o

root@qtrnas:~/google-authenticator-apache-module# make install

apxs2 -c mod_authn_google.c base32.c hmac.c sha1.c
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
mod_authn_google.c: In function 'ga_check_password':
mod_authn_google.c:310:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 7 has type 'apr_time_t' [-Wformat]
mod_authn_google.c: In function 'ga_get_realm_hash':
mod_authn_google.c:383:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 7 has type 'apr_time_t' [-Wformat]
mod_authn_google.c:404:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 8 has type 'apr_time_t' [-Wformat]
mod_authn_google.c: In function 'do_cookie_auth':
mod_authn_google.c:421:7: warning: format '%lu' expects argument of type 'long unsigned int', but argument 7 has type 'apr_time_t' [-Wformat]
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
/usr/share/apr-1.0/build/libtool --silent --mode=compile --tag=disable-static i686-linux-gnu-gcc -prefer-pic -DLINUX=2 -D_FORTIFY_SOURCE=2 -D_GNU_SOURCE -D_LARGEFILE64_SOURCEo
/usr/share/apr-1.0/build/libtool --silent --mode=link --tag=disable-static i686-linux-gnu-gcc -o mod_authn_google.la  -rpath /usr/lib/apache2/modules -module -avoid-version  o
sudo cp .libs/mod_authn_google.so /usr/lib/apache2/modules/
```

Una vez instalado el nuevo módulo (llamado ***mod\_authn\_google.so***) en el directorio de Apache, creamos el fichero de carga asociado ***mod\_authn\_google.so.load***, con la directiva de carga:

```auto
root@qtrnas:/etc/apache2/mods-available# <strong>nano mod_authn_google.so.load</strong>
```

```auto
Loadmodule authn_google_module /usr/lib/apache2/modules/mod_authn_google.so
```

Luego en el site que queremos activar la autenticación en dos pasos  (no es parte del tutorial explicar como crear y activar un site nuevo en apache */etc/apache2/site-available/*), incluimos las Directivas para activar el módulo:

```auto
AuthType Basic
   AuthName "Mi Test de 2-STEP"
   AuthBasicProvider "google_authenticator"
   Require valid-user
   GoogleAuthUserPath ga_auth
   GoogleAuthCookieLife 3600
   GoogleAuthEntryWindow 2
```

Ahora hay que configurar la sincronización de las claves autogeneradas para cada usuario que tenga que tener acceso. Para ello es necesario copiar el fichero *.google-authenticator* (propietario de cada usuario, por lo que estará en la */home/<usuario>/)*, a un nuevo directorio */etc/apache2/ga\_auth/* donde estarán todos con el nombre de usuario de acceso. Luego ajustamos permisos para que se pueda leer la información:

```auto
root@qtrnas:/etc/apache2/# mkdir ga_auth
root@qtrnas:/etc/apache2/ga_auth# cp /home/user1/.google_authenticator user1
root@qtrnas:/etc/apache2/ga_auth/# chmod 640 user1 && chown root:www-data user1
```

Ya solo queda reiniciar el servidor Apache y verificar el aceso.

```auto
sudo service apache2 restart
```

Cuando accedamos por web, nos aparecerá una ventana en la cual tendremos que introducir el usuario (en este ejemplo *user1*) y en password pondremos el código de acceso autogenerado por GoogleAuthenticator de nuestro móvil. Si pasamos de este nivel de seguridad, entraremos en la aplicación/site web, la cual puede tener su sistema de autenticación propio.
