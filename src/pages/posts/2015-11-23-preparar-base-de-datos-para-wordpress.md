---
layout: ../../layouts/post.astro
title: "Preparar base de datos para Wordpress"
pubDate: 2015-11-23
description: "Me apunto los pasos sencillos para preparar la base de datos antes de instalar Wordpress."
author: "akirasan"
isPinned: false
excerpt: "Me apunto los pasos sencillos para preparar la base de datos antes de instalar Wordpress."
image:
  src: "http://cleventy.com/wp-content/uploads/2014/03/wordpress-developer.jpg"
  alt: "Preparar base de datos para Wordpress"
tags: ["wordpress", "mysql"]
---

Me apunto los pasos sencillos para preparar la base de datos antes de instalar Wordpress.

Primero conectamos al servidor de mediante *root* y nos solicitará el password:

```auto
$ sudo mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 38
Server version: 5.5.46-0+deb7u1 (Debian)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

Hasta aquí todo sencillo ahora vamos con los comentados a ejecutar. A tener en cuenta:

* usuario: **wpuser**
* password: **wppassword**
* nombre de la base de datos: **wordpress**

```auto
mysql> CREATE DATABASE wordpress;
Query OK, 1 row affected (0.00 sec)
```

```auto
mysql> CREATE USER wpuser@localhost IDENTIFIED BY 'wppassword';
Query OK, 0 rows affected (0.00 sec)
```

```auto
mysql> GRANT ALL PRIVILEGES ON wordpress.* TO wpuser@localhost;
Query OK, 0 rows affected (0.00 sec)
```

```auto
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
```

Se acabó!!!

```auto
mysql> exit
Bye
```
