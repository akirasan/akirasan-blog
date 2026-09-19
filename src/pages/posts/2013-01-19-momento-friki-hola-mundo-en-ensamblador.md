---
layout: ../../layouts/post.astro
title: "Momento friki: \"hola mundo\" en ensamblador"
pubDate: 2013-01-19
description: "He tenido un pequeño momento friki de recuerdo de mis antiguas andanzas con la programación en ASM (ensamblador). Ese recuerdo me ha hecho p"
author: "akirasan"
isPinned: false
excerpt: "He tenido un pequeño momento friki de recuerdo de mis antiguas andanzas con la programación en ASM (ensamblador). Ese recuerdo me ha hecho p"
image:
  src: ""
  alt: "Momento friki: \"hola mundo\" en ensamblador"
tags: []
---

He tenido un pequeño momento friki de recuerdo de mis antiguas andanzas con la programación en ASM (ensamblador). Ese recuerdo me ha hecho pensar en intentar el típico "hola mundo" en ensamblador.

Siempre había programado en x86 bajo MS-DOS y recuerdo aquella interrupción para hacer casi de todo, la *int 21h*. Ahora, un aliciente mas ha sido que no tengo MS-DOS, sino Ubuntu y en 64bits (nada de 32bits), por lo que hay algunos pequeños cambios: por ejemplo la *int 21h* es la [*int 80h*](http://www.int80h.org/ "http://www.int80h.org/") (la que tiene los servicios de kernel).

Para empezar el compilador; he utilizado el [NASM](http://nasm.us/ "http://nasm.us/") por poner uno, aunque hay varios y la verdad es que no he tenido tiempo ni de comparar ni analizar nada,...

Creamos un fichero *holamundo.asm* con este código fuente:

```auto
section .text
    global _start               ;necesario para el linkado (ld)

_start:                         ;inicio del codigo

        mov     edx,txtlong             ;longitud del maxima del mensaje
        mov     ecx,texto               ;mensaje
        mov     ebx,1           ;file descriptor (stdout)
        mov     eax,4           ;numero de servicio (sys_write)
        int     0x80            ;interrupcion 80h (llama al kernel)

        mov     eax,1           ;numero de servicio (sys_exit)
        int     0x80            ;llamada al kernel otra vez

section .data
texto   db      'Hola mundo!!!',0xa     ;mensaje, acabado en null
txtlong equ     $ - texto                       ;length of our dear string
```

Para compilar el código tan simple como:

```auto
nasm -f elf64 holamundo.asm -o holamundo.o
```

Una vez compilado y generado el fichero objeto, hay que linkarlo. Para ello hay que utilizar el comando ld:

```auto
ld -o holamundo holamundo.o
```

Listo!!! compilado y linkado, ahora ya solo queda ejecutarlo:

Que maravilla!!! :)
