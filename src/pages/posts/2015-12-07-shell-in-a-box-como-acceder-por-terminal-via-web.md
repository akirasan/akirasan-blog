---
layout: ../../layouts/post.astro
title: "Shell in a box. Como acceder por terminal, pero vía web"
pubDate: 2015-12-07
description: "**Shellinabox** es una gran aplicación/utilidad que permite acceder vía terminal de forma remota y vía web, mediante SSL/TLS. *shellinabox*"
author: "akirasan"
isPinned: false
excerpt: "**Shellinabox** es una gran aplicación/utilidad que permite acceder vía terminal de forma remota y vía web, mediante SSL/TLS. *shellinabox*"
image:
  src: "/content/images/2015/12/shellinabox.jpg"
  alt: "Shell in a box. Como acceder por terminal, pero vía web"
tags: []
---

**Shellinabox** es una gran aplicación/utilidad que permite acceder vía terminal de forma remota y vía web, mediante SSL/TLS. *shellinabox* es un servicio que está escuchando por el puerto 4200 (por defecto), por lo tanto el acceso sería a la dirección **<http://localhost:4200>**. Si por ejemplo queremos acceder desde fuera de nuestra red, lo ideal es hacer foward del puerto, por ejemplo, el puerto 443 (https).

La instalación es muy sencilla en [Ubuntu](https://help.ubuntu.com/community/shellinabox), ya que es un paquete en la distribución.

```auto
sudo apt-get install shellinabox
```

Una de las cosas que podemos hacer, para publicar nuestro acceso hacia internet, es utilizar webproxy mediante Apache. Para ello podemos configurar un site con un fichero de configuración, pero primero necesitamos los siguientes componentes:

```auto
sudo apt-get install libapache2-mod-proxy-html apache2
```

Ahora activamos el módulo proxy\_http de Apache:

```auto
sudo a2enmod proxy_http
```

Ahora tenemos que cambiar configuración del *shellinabox*, que está en este directorio: */etc/default/shellinabox* y cambiamos la línea:

```auto
SHELLINABOX_ARGS="--no-beep"
```

...por esta otra línea

```auto
SHELLINABOX_ARGS="--no-beep --disable-ssl"
```

Ahora tenemos que generar, o modificar uno existente, un fichero de configuración del site que vamos a utilizar. Debe tener una pinta como esta:

```auto
ProxyRequests Off
<Proxy>
        Order deny,allow
        Allow from all
</Proxy>
 
<Location /shell/>
        ProxyPass http://localhost:4200/
        ProxyPassReverse http://localhost:4200/
</Location>
```

Activamos el site nuevo y reiniciamos todo:

```auto
sudo a2ensite <site_configurado>
sudo service shellinabox restart
sudo service apache2 reload
```

Ahora si accedemos al nuevo site, tendremos que hacerlo mediante el nombre que hemos definido en el proxy: **http://{dirección\_servidor\_web}/shell/**

![](/content/images/2015/12/shellinabox_ls.jpg#small)

###### UPDATE [09/12/2015]

Si vais a activar el acceso desde fuera de vuestra red, es decir, añadiendo una entrada NAT al router de tal forma que podáis acceder desde cualquier navegador conectado a internet, es interesante añadir algo de seguridad en el proceso de login, como por ejemplo [la autenticación en dos pasos (2-step) que nos ofrece Google Authenticator](http://www.akirasan.net/activar-google-authenticator-para-nuestro-acceso-ssh/). Eso si, no configurando la posibilidad de que NO pida el 2-step cuando estamos en red local, ya que shellinabox queda identificado como una conexión interna propia y no saltaría el 2-step (es el fichero */etc/security/access-local.conf*).
