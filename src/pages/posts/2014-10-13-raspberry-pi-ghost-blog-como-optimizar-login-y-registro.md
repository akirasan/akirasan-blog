---
layout: ../../layouts/post.astro
title: "Raspberry Pi + Ghost Blog. Como optimizar login y registro"
pubDate: 2014-10-13
description: "Uno de los problemas que podemos encontrarnos cuando utilizamos Ghost bajo una"
author: "akirasan"
isPinned: false
excerpt: "Uno de los problemas que podemos encontrarnos cuando utilizamos Ghost bajo una"
image:
  src: "https://cdn.tutsplus.com/mac/uploads/2013/10/ghostpi400.png"
  alt: "Raspberry Pi + Ghost Blog. Como optimizar login y registro"
tags: ["RaspberryPi", "Ghost"]
---

Uno de los problemas que podemos encontrarnos cuando utilizamos Ghost bajo una  
Raspberry Pi, es que el login o registro es muy lento y tiene un alto consumo de CPU (prácticamente se pone al 95% sostenido durante largo tiempo). Esto ocasiones en la mayoría de veces un mensaje de error y en otras una larga espera.

Este problema se debe básicamente a la librería que se utiliza para hacer hash del password (*bcryptjs*), que normalmente en un servidor normal no tarda prácticamente nada, pero con los recursos que tenemos en nuestra Raspberry Pi este algoritmo se hace demasiado pesado, por lo que vamos a cambiarlo por otro mas optimizado: *bcrypt*.

En el path donde tenemos instalado nuestro Ghost (en mi caso */var/www/ghost*, ejecutamos el siguiente comando:

```auto
sudo npm install bcrypt
```

Y obtendremos un log como este:

```auto
/
> bcrypt@0.8.0 install /var/www/ghost/node_modules/bcrypt
> node-gyp rebuild

gyp WARN EACCES user "root" does not have permission to access the dev dir "/root/.node-gyp/0.10.32"
gyp WARN EACCES attempting to reinstall using temporary dev dir "/var/www/ghost/node_modules/bcrypt/.node-gyp"
make: Entering directory '/var/www/ghost/node_modules/bcrypt/build'

CXX(target) Release/obj.target/bcrypt_lib/src/blowfish.o
CXX(target) Release/obj.target/bcrypt_lib/src/bcrypt.o
CXX(target) Release/obj.target/bcrypt_lib/src/bcrypt_node.o
SOLINK_MODULE(target) Release/obj.target/bcrypt_lib.node
SOLINK_MODULE(target) Release/obj.target/bcrypt_lib.node: Finished
COPY Release/bcrypt_lib.node
make: Leaving directory '/var/www/ghost/node_modules/bcrypt/build'
bcrypt@0.8.0 node_modules/bcrypt
├── bindings@1.0.0
└── nan@1.3.0
```

Ahora tenemos que reemplazar la antigua librería con la nueva, para ello editamos el fichero *user.js* :

```auto
sudo nano core/server/models/user.js
```

Buscamos y reemplazamos `bcrypt = require('bcryptjs')` por esto `bcrypt = require('bcrypt')`, donde indicamos la utilización la nueva librería.

Ahora ya solo queda reiniciar nuestro servicio Ghost y listo, ahora el login va a dejar de ser una tortura.

Referencia: [Hardware Hacks](http://c-mobberley.com/wordpress/2014/01/30/raspberry-pi-ghost-blog-slow-login-sign-up-fix/)
