---
layout: ../../layouts/post.astro
title: "ESPlorer: IDE de programación para ESP8266"
pubDate: 2015-05-17
description: "Si el  que vimos hace poco, no os acabó de convencer, aquí traigo un IDE para poder compilar y gestionar código sobre nuestro ESP8266. **Not"
author: "akirasan"
isPinned: false
excerpt: "Si el  que vimos hace poco, no os acabó de convencer, aquí traigo un IDE para poder compilar y gestionar código sobre nuestro ESP8266. **Not"
image:
  src: "/content/images/2015/05/IMG_20150413_222403.jpg"
  alt: "ESPlorer: IDE de programación para ESP8266"
tags: ["Linux", "IoT", "ESP8266"]
---

Si el [hola mundo](https://qtrsite.akirasan.net:84/programacion-directa-sobre-un-modulo-esp8266/) que vimos hace poco, no os acabó de convencer, aquí traigo un IDE para poder compilar y gestionar código sobre nuestro ESP8266. **Nota importante:** Para subir el firmware era necesario que el pin GPIO0 estuviera a GND, pero para subir scripts de Lua no debe estar a GND. Esto, como ya comentamos, es debido a que el GIPO0 indica el modo de boot de módulo.

El IDE que vamos a utilizar se llama **ESPlorer** y lo podéis [descargar desde aquí](http://esp8266.ru/esplorer/#download). Una de las ventajas que tiene es que es Java y por lo tanto crossplatform, así que lo podemos utilizar bajo linux sin problemas (yo lo utilizo bajo Ubuntu).

La *instalación* del ESPlorer (por llamarlo de alguna forma), no tiene misterio: descromprimir un .zip que trae un fichero llamado **ESPlorer.jar** y que lo ejecutamos con la MV de Java.

##### El bootloader del ESP8266

Lo primero que tenenemos que entender es que el módulo wifi ESP8266 cuando comienza su proceso de boot (entre otras cosas), lo primer que va a ejecutar será el script llamado **init.lua**. Por lo tanto lo primero que pensamos es que ahí es donde vamos a colocar toda nuestra lógica,...en parte es así, pero lo que se suele aconsejar es que no se incluya en este fichero el loop principal de nuestro programa (la verdad es que no he acabado de enter muy bien el porque, pero bueno,...vamos a seguir la recomendación).

##### Hola mundo!!!

Vamos a crear dos scripts lua: el primero será el fichero de inicio **init.lua** y luego un script que será invocado, llamado **configurar\_wifi.lua** y que va a ser el encargado de conectarse a nuestra wifi y montar un servidor http que atienda una petición por el puerto 80.

###### init.lua

Como hemos comentado antes este scripts será el que ejecute el boot del firmware del ESP8266 y por lo tanto será *el director* y nos marcará que scripts ejecutar. En este caso el código es muy sencillo, va a invocar a nuestro *"script estrella"*

```auto
dofile ("configurar_wifi.lua")
```

![](/content/images/2015/05/Selecci-n_041.jpg)

Creamos el script y le damos al botón *"Save to ESP"*. Básicamente va a enviar el código que hemos creado y guardarlo en la memoria flash del módulo, lo que significa que lo podremos ejecutar desde allí cuando queramos. Veremos como por consola ejecutar los diversos comandos para subir el código. Al finalizar lo ejecutará, pero dará un error porque no encontrará el script *configurar\_wifi.lua* que aún no hemos subido.

###### configurar\_wifi.lua

Llega la madre del cordero!!!. Vamos a ver en el código dos partes diferenciadas:

1. Conexión a la wifi
2. Configurar servidor de respuesta

```auto
print('Configurando wifi') 
wifi.setmode(wifi.STATION)
-- wifi config start
wifi.sta.config("<SSID>","<PASSWORD_WIFI>")
-- wifi config end

--local ip_asignada = wifi.sta.getip()
--print('IP asignada: ' .. ip_asignada)

-- un simple servidor http
print ('Configurando el servidor holamundo')

srv=net.createServer(net.TCP)

srv:listen(80,function(conn)
    conn:on("receive",function(conn,payload)
      print(payload)                                --peticion recibida
      local mensaje = "<h1>Hola mundo!!!</h1>"      --mensaje de respuesta
      local longitud = string.len(mensaje)          --calculamos la longitud de la respuesta (protocolo http)
      conn:send("HTTP/1.1 200 OK\r\n")                               --cabecera http
      conn:send("Content-Length:" .. tostring(longitud) .. "\r\n")   --longitud del contenido
      conn:send("Connection:close\r\n\r\n")                          --fin de la respuesta
      conn:send(mensaje)
    end)

    conn:on("sent",function(conn)
       conn:close()
    end)
end)
print ('Ya se puede consultar por http el holamundo!!!')
```

Creamos el script y le damos al botón *"Save to ESP"*. El script se ejecutará, así que ya tendrémos nuestro servidor *"hola mundo"* funcionando,...si le hacemos un reset, veremos como al arrancar, ejecutará el script *init.lua* el cual llamará a este código.

![](/content/images/2015/05/Selecci-n_042.jpg)

Vayamos por partes del código:

La conexión a nuestra wifi la realizaremos con la instrucción `wifi.sta.config` donde pasaremos el SSID de nuestra wifi y el password. Podemos conectarnos por DHCP (nos asignará un IP dinámica) o fijar una IP dentro de nuestro rango:

**DHCP**

```auto
wifi.setmode(wifi.STATION)
-- wifi config start
wifi.sta.config("<SSID>","<PASSWORD_WIFI>")
-- wifi config end
```

**IP fija**

```auto
 wifi.sta.config("SSID","PASSWORD_WIFI")
 wifi.sta.connect()
 wifi.sta.setip({ip="DIRECCION_IP",netmask="NETMASK",gateway="DIRECCION_GATEWAY"})
```

Una vez configurado nuestro acceso a la red wifi, toca declarar un servidor que atienda las peticiones por el puerto 80:

```auto
srv=net.createServer(net.TCP)
srv:listen(80,function(conn)
```

Creamos con `net.createServer(net.TCP)` un servidor tipo TCP. Esto nos devolverá un objeto `net.server` y utilizamos el método `listen` para definir que escuche por el puerto 80. La función con socket `net.socket` que definimos será *conn*. Este socket tendrémos que registrar la llamada a un evento (que puede ser: "connection", "reconnection", "disconnection", "receive", "sent") mediante `net.socket:on()`. En este caso para el método *"receive"* (que se ejecuta cuando recibe una petición) ejecutaremos lo siguiente:

`conn:on("receive",function(conn,payload)`

Hacemos un `print`del parámetro *payload* que básicamente es para ver la petición que nos ha llegado, así veremos por la consola del ESPlorer la petición HTTP que realizará el navegador:

`print(payload)`

Ahora vamos a preparamos el mensaje de respuesta, que será un *"hola mundo"* y por protocolo de http debemos pasar la logitud, por lo tanto la calculamos con la instrucción `string.len()`

```auto
local mensaje = "<h1>Hola mundo!!!</h1>"
local longitud = string.len(mensaje)
```

Ya solo queda enviar la respuesta mediante módulo `net.socket:send()`. La respuesta evidentemente tiene que ser en formato http y cumplir con el esquema de cabecera:

```auto
      conn:send("HTTP/1.1 200 OK\r\n")
      conn:send("Content-Length:" .. tostring(longitud) .. "\r\n")
      conn:send("Connection:close\r\n\r\n")
      conn:send(mensaje)
    end)
```

Si todo está running, ahora podemos acceder desde un navegador a la IP que tenga nuestro ESP8266 y ver la respuesta de un "Hola mundo" vía web:

![](/content/images/2015/05/Selecci-n_043.jpg)

En el screenshot de arriba he marcado lo que aparece por la consola del ESPlorer, ahí tenemos la petición que nos llega y el final de la respuesta.

Pues eso es todo. Ahora a seguir jugando y preparando algo con cara y ojos para el Internet de las cosas IoT
