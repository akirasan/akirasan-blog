---
layout: ../../layouts/post.astro
title: "Lectura Monitor de Consumo Eléctrico online (CC128) desde Ubuntu (consola)"
pubDate: 2011-03-17
description: "Ha llegado a mis manos (de forma temporal) un medidor de consumo eléctrico en tiempo real. Es de la familia de los . La instalación a nivel"
author: "akirasan"
isPinned: false
excerpt: "Ha llegado a mis manos (de forma temporal) un medidor de consumo eléctrico en tiempo real. Es de la familia de los . La instalación a nivel"
image:
  src: ""
  alt: "Lectura Monitor de Consumo Eléctrico online (CC128) desde Ubuntu (consola)"
tags: []
---

Ha llegado a mis manos (de forma temporal) un medidor de consumo eléctrico en tiempo real. Es de la familia de los [CurrentCost CC128](http://www.currentcost.com/product-envi.html "http://www.currentcost.com/product-envi.html"). La instalación a nivel electrica es sencilla y no tiene mucho secreto (lo podéis ver en la propia web de [Current Cost](http://www.currentcost.com/ "http://www.currentcost.com/")).

Ver el consumo de forma online nos puede ayudar sobretodo a concienciarnos en el tema de consumo electrico, así como llevar el control del gasto realizado y que luego no nos time la compañía electrica. El propio dispositivo receptor es capaz de almacenar unos 7 años de información diaria, pero si lo que queremos es tener mayor detalle, podemos conectarlo al PC y recuperar la información en tiempo real.

A continuación voy a explicar como hacerlo por consola desde Ubuntu. Al final lo que tendrémos será un script que nos mostrará la información recibida del consumo en tiempo real (cada 5seg).

**Prerequisitos**: Hay que tener instalado el Perl y el módulo Device::SerialPort

Instalar módulo Device::SerialPort en Perl (suponiendo que ya tenéis Perl instalado): Desde una consola ejecutamos el siguiente comando como *root*:

> ```auto
> perl -MCPAN -e shell
> ```

Cuando ha acabado las verificaciones y cosas raras, ejecutamos el comando para instalar en módulo:

> ```auto
> install Device::SerialPort
> ```

Cuando finaliza, salimos com *exit*.

Ahora hay que crear el siguiente script, el cual leerá la salida que emite el receptor cada 5 seg y la interpreta, de tal forma que nos mostrará los watios y la temperatura en ese momento. Lo único que debemos tener en cuenta es el puerto USB por el que está conectado el receptor, en este caso en /dev/ttyUSB0 (marcado en rojo):

```auto
 #!/usr/bin/perl -w
 # Reads data from Current Cost and e.on energy monitor devices via serial port.
 use strict;
 use Device::SerialPort qw( :PARAM :STAT 0.07 );
 my $PORT = "<span style="color: #ff0000;">/dev/ttyUSB0</span><span style="font-family: terminal, monaco;">";
 my $ob = Device::SerialPort-&gt;new($PORT);
 $ob-&gt;baudrate(57600);
 $ob-&gt;write_settings;
 open(SERIAL, "+&gt;$PORT");
 while (my $line = &lt;SERIAL&gt;) {
  if ($line =~ m!&lt;tmpr&gt; *([\-\d.]+)&lt;/tmpr&gt;.*&lt;watts&gt;0*(\d+)&lt;/watts&gt;!) {
  my $temp = $1;
  my $watts = $2;
  print "$watts, $temp\n";
  }</span>
 }
```

El resultado cuando se ejecuta este script es algo como esto:

```auto
akirasan@akirasan-desktop:~/currentcost# ./consumo.sh
 2432, 19.9
 2443, 19.9
 2423, 19.8
 2435, 19.8
 1074, 19.8
 969, 19.9
 964, 19.9
 990, 19.9
 2436, 19.9
 2501, 19.8
 2460, 19.8
 2449, 19.8
 2458, 19.8
 2440, 19.8
 2449, 19.8
```

Y si ejecutamos un *cat* contra el device básicamente recogemos la misma información:

```auto
akirasan@akirasan-desktop:~/currentcost# cat /dev/ttyUSB0
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:44:42</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02455</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:44:48</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02474</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:44:54</time><tmpr>19.9</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02472</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:06</time><tmpr>19.9</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>01020</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:12</time><tmpr>19.9</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>01041</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:18</time><tmpr>19.9</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>01021</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:24</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02400</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:30</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02318</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:37</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02472</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:43</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02478</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:49</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02468</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:45:55</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02441</watts></ch1></msg>
<msg><src>CC128-v0.11</src><dsb>00003</dsb><time>09:46:02</time><tmpr>19.8</tmpr><sensor>0</sensor><id>00077</id><type>1</type><ch1><watts>02444</watts></ch1></msg>
```

Esto es un primer paso para comprobar y recoger la información online de una forma sencilla y por consola, pero si lo que queremos es almacenar y analizar la información de una forma mas cómoda (representación gráfica, históricos, etc,...) podemos utilizar una aplicación web como [energyathome](http://code.google.com/p/energyathome/ "http://code.google.com/p/energyathome/"), la cual requiere un entorno LAMP (Linux+Apache+MySQL+PHP) y Phyton para mostrarnos mas amigablemente la información que está recogiendo del receptor.

Descarga del script en Perl aquí: <http://pastebin.com/eksBABuD>

Información de referencia de ayuda:

<http://www.linuxuk.org/2008/12/currentcost-and-ubuntu/>

<http://www.jibble.org/currentcost/>
