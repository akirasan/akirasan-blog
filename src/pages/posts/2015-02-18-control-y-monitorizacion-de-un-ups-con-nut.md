---
layout: ../../layouts/post.astro
title: "Control y monitorización de un UPS con NUT"
pubDate: 2015-02-18
description: "Vamos a ver como configurar el control y monitorización de un UPS/SAI desde linux mediante la herramienta . Hay que decir, que en este post"
author: "akirasan"
isPinned: false
excerpt: "Vamos a ver como configurar el control y monitorización de un UPS/SAI desde linux mediante la herramienta . Hay que decir, que en este post"
image:
  src: ""
  alt: "Control y monitorización de un UPS con NUT"
tags: ["Linux", "Python", "NUT", "UPS"]
---

Vamos a ver como configurar el control y monitorización de un UPS/SAI desde linux mediante la herramienta [NUT Network UPS Tools](http://www.networkupstools.org/). Hay que decir, que en este post vamos a tratar los eventos generados de una forma diferente y totalmente propia, es decir, le vamos a dar un valor a la monitorización para que nos **informe mediante tweets de los eventos** y además podamos **realizar un shutdown controlado** de los equipos que queramos. Para ello vamos a crear un script propio en Python.

También para rizar el rizo y pasar de GUI's, nos **vamos a montar un servidor Flask para ver el estado de nuestro UPS vía web**.

![](/content/images/2015/02/salicru_ups_sai.png)

### Instalación y configuración de NUT

La instalación de NUT es muy sencilla, el tema potente viene en la configuración:

```auto
sudo apt-get install nut
```

Tras la instalación en el path `/etc/nut` tendremos una serie de ficheros de configuración, cinco en concreto que vamos a tocar lo mas simple posible:

```auto
akirasan@qtrnas:/etc/nut⟫ ll
total 64
drwxr-xr-x   2 root nut   4096 feb 15 17:09 ./
drwxr-xr-x 155 root root 12288 feb 17 23:15 ../
-rw-r-----   1 root nut   1544 feb 14 18:47 nut.conf
-rw-r-----   1 root nut   4636 feb 14 18:42 ups.conf
-rw-r-----   1 root nut   4647 feb 14 18:51 upsd.conf
-rw-r-----   1 root nut   2218 feb 16 13:12 upsd.users
-rw-r-----   1 root nut  16340 feb 18 14:21 upsmon.conf
-rw-r-----   1 root nut   4346 feb 15 17:40 upssched.conf
```

**nut.conf**

Le modificaremos el valor de MODE a **MODE=standalone**

**ups.conf**

Vamos a definir una entrada para nuestro UPS, le daremos un nombre identificativo `[salicru]` y definiremos el driver de conexión, puerto y una descripción:

```auto
[salicru]
  driver = blazer_usb
  port = auto
  desc = “Salicru SPS ONE”
```

**upsd.conf**

La configuración del demonio que nos permitirá acceder a la información de nuestro UPS y ejecutarle comandos, ya sea por comandos o mediante una aplicación externa. Simplemente necesitamos estas lineas, veremos que el puerto de conexión es **3493** normalmente es el puerto standard:

```auto
MAXAGE 15
STATEPATH /var/run/nut
LISTEN 127.0.0.1 3493
MAXCONN 1024
```

**upsd.users**

Configuración del usuario de acceso al UPS. Aquí definiremos que usuarios y autorizaciones le asignamos. Como hemos dicho lo haremos fácil, así que vamos a definir un solo usuario llamado **admin** y le vamos a dar permiso para ejecutar todos los comandos con *instcmds=ALL*.

```auto
[admin]
    password = admin1234
    actions = SET
    instcmds = ALL
```

**upsmon.conf**

Llega uno de los mas raros de configurar,...a ver si lo puedo hacer simple: Básicamente este fichero de configuración es el del monitorizador del estado de nuestro UPS. Tiene muchas directivas que pueden configurarse, pero lo básico va a ser esto:

Número de UPS conectadas, por normal general será 1:

```auto
MINSUPPLIES 1
```

La conexión hacia nuestro UPS, va en relación a lo que hemos configurado en el fichero **ups.conf**, **upsd.conf** y **upsd.users**

```auto
MONITOR salicru@localhost 1 admin admin1234 master
```

Scripts a ejecutar cuando se detecta un evento en el UPS. A parte de invocar este scripts (que puede ser bash, python o lo que queramos) le pasa como parámetro el evento, así podemos recuperarlo en el programa.

```auto
NOTIFYCMD "python /<path>/UPSwebmon/control_ups.py"
```

Definir que tipo de acciones hacer cuando se detecta un evento, por ejemplo, para el evento ONLINE (el UPS se conecta a la corriente), escribimos en el *log* (**syslog**) del sistema mediante **SYSLOG**, también informamos por pantalla (terminales conectados) con el **WALL**, y por último ejecutamos el script definido en este mismo fichero por **NOTIFYCMD**. Básicamente esto es lo que definimos aquí:

```auto
NOTIFYFLAG ONLINE   SYSLOG+WALL+EXEC
NOTIFYFLAG ONBATT   SYSLOG+WALL+EXEC
NOTIFYFLAG LOWBATT  SYSLOG+WALL+EXEC
NOTIFYFLAG FSD      SYSLOG+WALL+EXEC
NOTIFYFLAG COMMOK   SYSLOG+WALL+EXEC
NOTIFYFLAG COMMBAD  SYSLOG+WALL+EXEC
NOTIFYFLAG SHUTDOWN SYSLOG+WALL+EXEC
NOTIFYFLAG REPLBATT SYSLOG+WALL+EXEC
NOTIFYFLAG NOCOMM   SYSLOG+WALL+EXEC
NOTIFYFLAG NOPARENT SYSLOG+WALL+EXEC
```

Luego para los anteriores eventos, definimos el mensaje que vamos a escribir en **SYSLOG**, en el **WALL** y lo que vamos a enviar como parámetro al script que hemos definido en **NOTIFYCMD** con el **EXEC**

```auto
NOTIFYMSG ONLINE      "ONLINE"
NOTIFYMSG ONBATT      "ONBATT"
NOTIFYMSG LOWBATT     "LOWBATT"
NOTIFYMSG FSD         "UPS %s: forced shutdown in progress"
NOTIFYMSG COMMOK      "Comunicación establecida con el UPS %s"
NOTIFYMSG COMMBAD     "Comunicación perdida con el UPS %s"
NOTIFYMSG SHUTDOWN    "Comienza el Auto logout y shutdown"
NOTIFYMSG REPLBATT    "La batería del UPS %s necesita ser reemplazada"
NOTIFYMSG NOCOMM      "UPS %s no disponible"
NOTIFYMSG NOPARENT    "upsmon parent process died - shutdown impossible"
```

**Ejemplo:** Si se detecta un evento **COMMBAD** (pérdida de conexión con el UPS):

* Se va a escribir una linea en el *syslog* (SYSLOG) con este texto `Comunicación perdida con el UPS salicru`
* Veremos este texto en los terminales abierto (WALL)
* Ejecutará el script de esta forma (EXEC) `python /<path>/UPSwebmon/control_ups.py 'Comunicación perdida con el UPS salicru'`

Apunte para luego: veréis que para los eventos de **ONLINE**, **ONBATT** y **LOWBATT**, he puesto el mismo texto, esto es básicamente para simplificar luego el script que va a atender que hacer.

Hay parámetros ya definidos de forma standard que yo no he tocado, como estos:

```auto
POLLFREQ 5
POLLFREQALERT 5
HOSTSYNC 15
DEADTIME 15
POWERDOWNFLAG /etc/killpower
RBWARNTIME 43200
NOCOMMWARNTIME 300
FINALDELAY 5
```

### Instalación y configuración de nuestro monitor de eventos

Aquí vamos a diferenciar los dos scripts que he creado:

* Control de eventos: **control\_\_\_ups.py**, que es el script que llamará el *upsmon* (definido en el anterior apartado en la variable **NOTIFYCMD**
* Monitorización web: **web\_\_\_control\_\_\_ups.py**, que invocará un servidor web Flask y los ficheros de *templates* en HTML

Así lo tengo:

```auto
akirasan@qtrnas:/akirasan/UPSwebmon# ll
total 24
drwxrwxr-x 4 akirasan akirasan 4096 feb 18 14:13 ./
drwxr-xr-x 8 akirasan akirasan 4096 feb 17 22:54 ../
-rw-rw-r-- 1 akirasan akirasan 1458 feb 18 14:19 control_ups.py
drwxrwxr-x 2 akirasan akirasan 4096 feb 16 23:27 templates/
-rw-r--r-- 1 akirasan akirasan 2375 feb 18 14:13 web_control_ups.py
```

Primero vamos a necesitar [PyNUT](https://github.com/networkupstools/nut/tree/master/scripts/python), una librería en Python para poder hacer consultas sobre el estado del UPS, tanto en el script de control como en la información web que mostramos.

La instalación de PyNUT la tenemos que hacer incluyendo en el directorio `/usr/share/python-suport/python-pynut/` la librería que nos descargamos desde *github*. Algo así:

```auto
wget https://raw.githubusercontent.com/networkupstools/nut/master/scripts/python/module/PyNUT.py
```

Ahora ya podemos utilizarlo desde nuestros scripts. Ya veremos como se invoca y utiliza, aunque no es el objetivo principal de este post.

**control\_\_ups.py**

Veamos que hace este script. Como hemos dicho antes, su función principal va a ser la de recoger los eventos que detecte el *upsmon* y realizar una tweet para informarnos. Si no sabes como generer tweets desde Linux aquí tienes un post donde lo explicaba: [Raspberry generando tweets con Python](http://www.akirasan.net/raspberry-generando-tweets-con-python/), ya que no vamos a entrar en detalle de esta función.

Vamos a importar las librerías necesarias y vamos a definir un par de constantes: el nombre de nuestro UPS y el tiempo (en segundos) de espera hasta comenzar el shutdown controlado cuando el UPS entra en modo batería (esto cambiará en función de la autononía del UPS o de lo que tardemos en cerrar nuestro/s sistema/s):

```auto
import tweepy
import time
import sys
import PyNUT

DELAY_SHUTDOWN = 10  #Tiempo de espera antes de apagar sistema cuanto esta en bateria
UPS_NOMBRE = "salicru"
```

Primero vamos a recoger el evento/texto que nos pasa *upsmon* (lo que definimos en las variables **NOTIFYMSG** del fichero de configuración **upsmon.conf**) y lo enviamos a Twitter con la función *comunicar\_tweet()*

```auto
#  main ---------
evento_sai = sys.argv[1]
comunicar_tweet("["+evento_sai+"]")   #Tweet del estado recibido por el upsmonitor
```

Si vemos que el evento/texto es un *ONBATT*, significa que acabamos de peder la alimentación eléctrica y el UPS está en modo batería, así que comunicamos que estamos a la espera...

```auto
if evento_sai == "ONBATT":
  comunicar_tweet("Espero antes del apagado...")
  time.sleep(DELAY_SHUTDOWN)
```

Transcurrido el DELAY que hemos definido, consultamos la variable del UPS llamada *"ups.status"*, la cual nos dirá si aún estamos en módo batería o ya disponemos de alimentación:

```auto
  #Verificamos si tras la espera el UPS sigue en modo bateria
  nut = PyNUT.PyNUTClient( debug=True )
  result = nut.GetUPSVars( UPS_NOMBRE )
  if result['ups.status'] == "OB":  #OB= ONBATT    OL= ONLINE
    comunicar_tweet(time.strftime("%H:%M ")+"Comienza el proceso de apagado...")
    apagar_sistemas()
  else:
   comunicar_tweet("Cancelado el apagado controlado, sistema UPS vuelve a estar ONLINE")
```

Evidentemente si estamos en modo batería (OB), comunicamos vía tweet que vamos a comenzar con el cierre controlado de los sistemas. Veréis que no hay nada desarrollado en la función *apagar\_sistemas()*, ahí que queda uno implemente su estratégia de shutdown controlado. Si por el contratio el UPS ha recuperado la alimentación no ejecutamos nada y finalizamos nuestro script.

**web\_\_\_control\_\_\_ups.py**

Al igual que con el anterior script, en este no vamos a explicar el funcionamiento de Flask, aquí tenéis un post relacionado [Flask un webserver basado en Python](http://www.akirasan.net/raspberrypi-webserver-python/).

```auto
import PyNUT
from flask import Flask
from flask import render_template

UPS_NOMBRE = "salicru"

#webserver
app = Flask(__name__)
```

Lo que encontraremos en este scripts es la definición de dos funciones de Flask para los templates *index.html* y *update.html*. El primero de ellos es muy simple y no tiene funcionalidad.

```auto
@app.route("/")
def index():

  return render_template('index.html', **locals())
```

El segundo también es muy sencillo, básicamente traslada los valores de todas la variables de nuestro UPS al template *update.html*, para ello volvemos a utilizar la librería PyNUT y conocer el varlos de todas las variables.

```auto
@app.route("/update")
def update():
  nut = PyNUT.PyNUTClient( debug=False )
  result = nut.GetUPSVars( UPS_NOMBRE )

  v_battery_charge = result['battery.charge']
  v_battery_voltage = result['battery.voltage']
  v_battery_voltage_high = result['battery.voltage.high']
  v_battery_voltage_low = result['battery.voltage.low']
...
...
...
  v_ups_type = result['ups.type']
  v_ups_vendorid = result['ups.vendorid']

  return render_template('update.html', **locals())
```

Para conocer las variables que tiene tu UPS puedes ejecutar el comando `upsc <nombre_ups>` (supongo que pueden variar en función del modelo/marca)

Como resultado tendréis una tabla con los datos del UPS (muy simple):

![ups webmonitor](/content/images/2015/02/ups_webmonitor.jpg)

Solo tened en cuenta que en el html estoy utilizando [Bootstrap](http://getbootstrap.com/) y [JQuery](http://jquery.com/) por lo que tendrá que tener acceso web para descargarse estas librerías, a menos que lo descargueis en local y lo dejéis accesible para Flask.

Descarga los ficheros de ejemplo:

[control\_\_\_ups.py](http://akirasan.net/content/images/2015/02/control_ups.py)

[web\_\_\_control\_ups.py](http://akirasan.net/content/images/2015/02/web_control_ups.py)

[index.html](http://akirasan.net/content/images/2015/02/index.html)

[update.html](http://akirasan.net/content/images/2015/02/update.html)
