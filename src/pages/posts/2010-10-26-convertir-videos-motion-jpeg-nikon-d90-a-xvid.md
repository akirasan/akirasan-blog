---
layout: ../../layouts/post.astro
title: "Convertir vídeos Motion JPEG (Nikon D90) a XViD"
pubDate: 2010-10-26
description: "Desde que tengo la Nikon D90 no hago mas que comer gigas y gigas de disco,...y todo por las grabaciones de vídeo. A parte que es un formato"
author: "akirasan"
isPinned: false
excerpt: "Desde que tengo la Nikon D90 no hago mas que comer gigas y gigas de disco,...y todo por las grabaciones de vídeo. A parte que es un formato"
image:
  src: ""
  alt: "Convertir vídeos Motion JPEG (Nikon D90) a XViD"
tags: []
---

Desde que tengo la Nikon D90 no hago mas que comer gigas y gigas de disco,...y todo por las grabaciones de vídeo. A parte que es un formato de vídeo muy poco comprimido a día de hoy no hay reproductor multimedia que lo lea (Motion JPEG), así que he decido que a partir de ahora voy a convertir los vídeos a XViD. Como normalmente suelo tener varios en la SD, me he escrito un script para automatizar el proceso en Ubuntu.

Este es el script que me he montado para convertir los vídeos de la D90 con una calidad mas que aceptable y un tamaño muy reducido, mediante ***mencoder***:

```auto
#!/bin/bash
```

```auto
files=(`ls -1S *.AVI`)
```

```auto
mkdir ./tmp
```

```auto
for file in ${files[@]}
```

```auto
do
```

```auto
mv $file ./tmp/.
```

```auto
mencoder -oac mp3lame -lameopts cbr=128 -ovc xvid -xvidencopts bitrate=9600:threads=4 ./tmp/$file  -vf hqdn3d,softskip,harddup -o $file
```

```auto
touch -r ./tmp/$file $file
```

```auto
done
```

```auto
exit 0
```

Para utilizarlo únicamente tienes que crearte un fichero *.sh* (yo lo llamo *convertir.sh*) con permisos de ejecución (*chmod 777*) y este código dentro. Resumen de lo que hace el script:

* Selecciona todos los fichero AVI que hay en el directorio.
* Crea un directorio temporal (tmp) donde moverá todos los AVI originales.
* Convierte el AVI original y deja el fichero convertido con el mismo nombre y fecha de creación del original (para mi era un requisito mantener esta información para organizar conjuntamente fotos y vídeos).
