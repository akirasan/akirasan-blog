---
layout: ../../layouts/post.astro
title: "Activar Google Authenticator para nuestro acceso SSH"
pubDate: 2013-05-30
description: "Como configurar nuestro sistema linux para que las conexiones SSH tengan el 2-step o segundo paso de verificación  de forma opcional es sus"
author: "akirasan"
isPinned: false
excerpt: "Como configurar nuestro sistema linux para que las conexiones SSH tengan el 2-step o segundo paso de verificación  de forma opcional es sus"
image:
  src: ""
  alt: "Activar Google Authenticator para nuestro acceso SSH"
tags: []
---

Como configurar nuestro sistema linux para que las conexiones SSH tengan el 2-step o segundo paso de verificación [que tiene implementado Google](http://www.google.com/landing/2step/ "http://www.google.com/landing/2step/") de forma opcional es sus cuentas.

A tener en cuenta: Esta configuración es incompatible si está activado el acceso SSH por certificado. Por lo que si se quiere utilizar hay que desactivar esta opción (fichero *sshd\_config*). Está explicado mas adelante.

## **Instalación de Google Autenthicator**

Ejecutar el comando para instalar la librería PAM de google-authenticator.

```auto
sudo apt-get install libpam-google-authenticator
```

La librería se puede encontrar aquí <https://code.google.com/p/google-authenticator/> si no se dispone de los repositorios de Google, pero está incluido en PAM desde Ubuntu 11.10.

Con el usuario que se quiere activar el "segundo paso de verificación" (no con el root), ejecutar:

```auto
google-authenticator
```

Esto lanza una serie de preguntas y genera las claves. Contestar prácticamente a todos las preguntas de forma afirmativa.

Se generará una *secret key* para poder dar de alta esta cuenta en la aplicación Google Authenticator y una lista de códigos de emergencia para poder utilizarlos una vez en caso de necesidad y no tener disponible el generador de códigos (Google Authenticator).

Para introducir la cuenta nueva en la aplicación del movil se puede hacer de forma manual, introduciendo el la *secret key*, o utilizando la opción de scanear el código QR que aparece cuando ejecutamos *google-authenticator*.

Toda la información de *secret key* y códigos de emergencia quedan guardados en un fichero llamado ***.google\_authenticator*** en el */home* del usuario utilizado

## **Activar módulo de verificación para los logins por SSH**

Editar el fichero *sshd* de PAM para añadir el requisito nuevo

```auto
sudo nano /etc/pam.d/sshd
```

Ahora aquí hay dos posibles configuraciones que a mi me parecen interesantes:

**1. Configuración para activar 2-pass a todos los acceso SSH:**

```auto
auth required pam_google_authenticator.so
```

**2. Configuración para activar 2-pass solo a los accesos SSH externos a la red interna:**

Crear fichero con la configuración de acceso interno

```auto
sudo nano /etc/security/access-local.conf
```

Contenido del fichero *access-local.conf*. Hay que adaptar la ip a la de la red interna.

```auto
# only allow from local IP range
+ : ALL : 10.2.0.0/24
+ : ALL : LOCAL
- : ALL : ALL
```

Modificar el */etc/pam.d/sshd* con estas entradas y fichero de configuración anterior

```auto
# skip one-time password if logging in from the local network
auth [success=1 default=ignore] pam_access.so accessfile=/etc/security/access-local.conf
auth required pam_google_authenticator.so
```

## **Configurar servidor SSH**

Editar el fichero sshd\_config para tener soporte al módulo PAM

```auto
sudo nano /etc/ssh/sshd_config
```

Revisar la linea siguiente con este valor (si no existe hay que añadirla):

```auto
ChallengeResponseAuthentication yes
```

Si se tiene activado anteriormente el acceso por certificado hay que revisar las siguientes entradas. Las cuales tienen que tener estos valores:

```auto
RSAAuthentication no
PubkeyAuthentication no
PasswordAuthentication yes
```

Reiniciar el servicio SSH:

```auto
sudo service ssh restart
```

Ahora ya se puede acceder vía SSH y probar: Primero usuario y password (lo normal), y luego nos preguntará por el *Verification Code*, así que hay que tener a mano la aplicación Google Authenticator (para Android o IPhone).
