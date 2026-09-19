---
layout: ../../layouts/post.astro
title: "Instalación de Nodejs 4.4.3 en Raspberry Pi"
pubDate: 2016-05-06
description: "ese servidor con motor Javascript que lo está petando desde hace tiempo, lo podemos instalar en una Raspberry Pi (en todas sus versiones) de"
author: "akirasan"
isPinned: false
excerpt: "ese servidor con motor Javascript que lo está petando desde hace tiempo, lo podemos instalar en una Raspberry Pi (en todas sus versiones) de"
image:
  src: "/content/images/2016/05/nodejs-new-white-pantone.png"
  alt: "Instalación de Nodejs 4.4.3 en Raspberry Pi"
tags: ["nodejs", "rasp"]
---

[Nodejs](https://nodejs.org/en/) ese servidor con motor Javascript que lo está petando desde hace tiempo, lo podemos instalar en una Raspberry Pi (en todas sus versiones) de una forma bastante sencilla.

Para comenzar nos bajamos de [Node.js](https://nodejs.org/en/) la distribución adecuada en función del modelo de nuestra Raspberry Pi. Para ello tenemos que prestar atención a los ficheros **arm6l** o **armv7l**. Para diferenciarlos:

* Modelos Raspberry Pi A, B, B+

**node-*< versión >*-linux-armv6l.tar.gz**

* Modelos Raspberry Pi 2 B

**node-*< versión >*-linux-armv7l.tar.gz**

La lista de los ficheros las tenéis aquí <https://nodejs.org/dist/v4.4.3/> para la versión 4.4.3.

Para bajarnos la versión tan sencillo como hacer un *wget*. Por ejemplo para una Raspberry Pi A, B o B+, utilizaríamos la nomenclatura del fichero **arm6l**:

```auto
wget https://nodejs.org/dist/v4.4.3/node-v4.4.3-linux-armv6l.tar.gz
```

Luego descomprimimos el archivo con un *tar*, entramos en el directorio y copiamos todo el contenido sobre la ruta */usr/local/node*:

```auto
tar -xvf node-v4.4.3-linux-armv6l.tar.gz 
cd node-v4.4.3-linux-armv6l
sudo cp -R * /usr/local/
```

Esto es todo!!!, para verificar que lo tenemos instalado, tan sencillo como ejecutar un `node -v`:

```auto
~/node-v4.4.3-linux-armv6l $ node -v
v4.4.3
```
