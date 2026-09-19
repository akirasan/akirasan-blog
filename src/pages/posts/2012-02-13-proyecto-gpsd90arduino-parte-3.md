---
layout: ../../layouts/post.astro
title: "Proyecto GPS+D90+Arduino (Parte 3)"
pubDate: 2012-02-13
description: "Tercera parte  del proyecto: Ya tengo el *engendrillo* montado y funcionando en modo prototipo!!!. El arduino está programado para recibir y"
author: "akirasan"
isPinned: false
excerpt: "Tercera parte  del proyecto: Ya tengo el *engendrillo* montado y funcionando en modo prototipo!!!. El arduino está programado para recibir y"
image:
  src: ""
  alt: "Proyecto GPS+D90+Arduino (Parte 3)"
tags: []
---

Tercera parte  del proyecto: Ya tengo el *engendrillo* montado y funcionando en modo prototipo!!!. El arduino está programado para recibir y transformar la información que recibe vía bluetooth el GPS (Holux M-241) y envía por el conector a la cámara (Nikon D90). Antes de pasar a la teoría aburrida del código, aquí os dejo alguna foto:

[![](http://static.zooomr.com/images/10159446_ed218fd590.jpg "http://static.zooomr.com/images/10159446_ed218fd590.jpg")](http://static.zooomr.com/images/10159446_ed218fd590_b.jpg) [![](http://static.zooomr.com/images/10159682_7c90abfcbf.jpg "http://static.zooomr.com/images/10159682_7c90abfcbf.jpg")](http://static.zooomr.com/images/10159682_7c90abfcbf_b.jpg)

Le he puesto tres led's para conocer el estado (rojo = estado del bluetooth, verde = operativo, amarillo = proceso de configuración). En el código veréis referencias y funciones para tratar los leds:

[![](http://static.zooomr.com/images/10159507_1b6d7ebd82.jpg "http://static.zooomr.com/images/10159507_1b6d7ebd82.jpg")](http://static.zooomr.com/images/10159507_1b6d7ebd82_b.jpg)

En esta parte prácticamente es todo programación. He colocado bastantes comentarios en el sketch y el código está muy modularizado para simplificar y entender las acciones. Como podéis ver, en las funciones estandars ***setup()*** y **loop()**, es muy simple:

```auto
void setup()
{
  configurar_pines();
  check_leds();            //secuencia de chequeo visual de los leds
  pinMode(ledR,INPUT);     //redefinimos pin del led rojo para que funcione como led del estado del bluetooth
  configurar_bluetooth();
  onled(ledV);
}

void loop()  

{  

while (bt.available()){  

i = 0;  

buffer[i] = (char)bt.read();  

if (buffer[i]=='$'){        //inicio sentencia  

leer_comando();           //lee hasta la "," i=","  

validar_comando();        //verifica si es $GPGGA o $GPRMC  

if (_GPGGA || _GPRMC){  

procesar_comando();     //comando válido  

i = 0;  

}  

}//fin inicio sentencia  

}//fin bt.available  

}//fin loop
```

  
...pero, hay un par de puntos que hay que tener en cuenta:

1. Configurar el módulo de bluetooth para conectarse al GPS, es decir, que cuando arranquemos el bluetooth sea capaz de emparejarse con el GPS.
2. Tratar las sentencias NMEA que envía el GPS para que las reconozca la Nikon D90. Hay un par de temillas a tener en cuenta :)

**Configurar módulo bluetooth**

Antes de utilizarlo, es recomendable conectarlo vía adaptador USB-UART(TTL) y entrar en modo AT para configurar la velocidad por defecto, que es de 9600 a 4800. Velocidad con la que trabajaremos, ya que la Nikon D90 utiliza esta velocidad para recibir los datos. Esta parte es simple, únicamente necesitas un adaptador serial-USB y conectarte al puerto COM. Puedes utilizar el mismo monitor de serial que viene en el IDE de Arduino o utilizar el Putty u otros,...(recordad que para entrar en modo AT hay que seguir lo que se comenta en el siguiente párrafo para entrar en modo AT). [Comandos AT que se pueden utilizar](http://www.cutedigi.com/pub/Bluetooth/BMX_Bluetooth_quanxin.pdf). Necesitaréis algo como esto:

[![](http://static.zooomr.com/images/10160520_23f30bf6a9_m.jpg "http://static.zooomr.com/images/10160520_23f30bf6a9_m.jpg")](http://static.zooomr.com/images/10160520_d66fdc2995_o.jpg)

Este módulo conversor con chip CP2102 es bastante barato y en ebay lo podéis encontrar por unos 3$ (con gastos incluidos).

Para que el módulo BT (bluetooth) se conecte al GPS hay que inicializarlo en modo AT, es decir, que acepte los comandos de configuración AT. Para ello hay que poner en HIGH (tensión) el pin 34 (o el PIO11) y luego poner encender el módulo. Esto hará que el módulo BT acepte comandos AT.

Básicamente le enviaremos los comandos de fijar a rol master, fijamos el password o pincode (del módulo GPS), emparejamiento y vinculación:

```auto
void iniciar_BT_modoAT(){
  // Entrar en modo AT - pin34_HIGH + power
  flashled(ledA,2);
  digitalWrite(bt_PowerPin, HIGH);
  delay(4000);
  flashled(ledA,1);
  digitalWrite(bt_KEYPin, HIGH);
  delay(1000);
  bt.listen();
}

void finalizar_BT_modoAT(){
  // Quitamos power al pin34
  digitalWrite(bt_KEYPin, LOW);
}

void configurar_bluetooth()
{
  String result, bt_pincode, bt_mac;

  flashled(ledA,3);
  Serial.println("Inicializando modulo BT...");

  iniciar_BT_modoAT();

  // Entramos en modo master============================== se supone que debe estar en role=1
  flashled(ledA,2);
  Serial.println("-----------------role");
  enviar_comandoAT("AT+ROLE=1", &result);
  Serial.println(result);

  // Fijamos el pin==============================
  flashled(ledA,2);
  bt_pincode = BT_pincode;
  Serial.println("-----------------password");
  enviar_comandoAT("AT+PSWD="+bt_pincode, &result);
  Serial.println(result);

  // Fijamos el emparejamiento==============================
  flashled(ledA,2);
  bt_mac = BT_MAC;
  Serial.println("-----------------pair");
  enviar_comandoAT("AT+PAIR="+bt_mac+",20", &result);
  Serial.println(result);

  // Vinculamos al GPS==============================
  flashled(ledA,2);
  Serial.println("-----------------link");
  enviar_comandoAT("AT+LINK="+bt_mac, &result);
  Serial.println(result);

  finalizar_BT_modoAT();
}

void enviar_comandoAT(String cmd, String *resultado)
{
  String result = "";

  Serial.print("\n-----llamamos al comando "+cmd+" --> ");

  for (int i2=0; i2<10 ; i2++){
    while (bt.available()){
      Serial.print((char)bt.read());
    }
  }

  bt.print(cmd+"\r\n");
  delay(3000);

  if (bt.available()){
    while(bt.available())
    {
      result.concat((char)bt.read());
    }
  }
  Serial.print("------resultado:"+result);
  *resultado = result;
}
```

En el sketch está indicado, pero hay que tener en cuenta un detalle con el número de la MAC del dispositivo al que nos tenemos que conectar, es decir, hay que adaptar su formato. Por ejemplo una MAC del estilo ***00:1b:c1:06:1b:6b*** se debe enviar así *1B,C1,61B6B*.

**Adaptación de sentencias NMEA**

[De todas las sentencias NMEA](http://www.gpsinformation.org/dale/nmea.htm "nmea") que envía el GPS, la Nikon D90 únicamente reconoce (o simplemente utiliza) dos: **$GPGGA** y **$GPRMC**. En mi caso, de estas sentencias, la **$GPGGA** requiere además una pequeña adaptación para que la D90 la entienda (tal vez sería bueno una actualización de firmware de la cámara,...). Así que ojo con el módulo de GPS que utilizáis, porque tal vez **no sea necesaria** esta adaptación,...o si,...hay que verificarlo. Para ello conéctalo por un puerto COM y monitoriza sus sentencias NMEA.

Ejemplo de una trama en formato NMEA:

```auto
$GPGGA,152848.000,4199.9999,N,00209.9999,E,1,7,1.46,10.8,M,51.3,M,,*6B
$GPGSA,A,3,02,10,07,13,05,04,23,,,,,,1.72,1.46,0.90*0A
$GPGSV,3,1,12,10,79,346,22,07,67,128,16,13,51,047,27,04,50,216,28*72
$GPGSV,3,2,12,02,48,279,33,08,43,179,,23,23,063,30,05,21,304,31*7C
$GPGSV,3,3,12,16,08,044,,26,03,250,,20,02,120,,49,,,*4B
$GPRMC,152848.000,A,4199.9999,N,00209.9999,E,0.31,0.27,020212,,,A*67
$GPVTG,0.27,T,,M,0.31,N,0.57,K,A*38
```

Sentencia $GPGGA. Dos son las modificaciones que con el GPS Holux M-241 hay que hacer:

1. Número de satélites
El número de satélites tiene que ser de dos dígitos, por lo que si recibimos un número inferior a 10, tendremos que añadir un 0 (cero) por delante. Por ejemplo, esta sentencia:

$GPGGA,152848.000,4199.9999,N,00209.9999,E,1,**7**,1.46,10.8,M,51.3,M,,\*6B

Se tiene que sustituir por:

$GPGGA,152848.000,4199.9999,N,00209.9999,E,1,**07**,1.46,10.8,M,51.3,M,,\*6B

Esta info la descubrí por propia experiencia la cual confirmé en esta web <http://we.easyelectronics.ru/upgrade-repair/analog-nikon-gps.html> (ojo!!! que el contenido **está en ruso**).  
- Altitud
  
Algo parecido al anterior punto le pasa al número que marca la altitud en metros. Hay que hacer lo mismo, rellenar con 0 (ceros) hasta completar un número con cuatro digitos. Un ejemplo:

$GPGGA,152848.000,4199.9999,N,00209.9999,E,1,07,1.46,**10.8**,M,51.3,M,,\*6B

Debe quedar así:

$GPGGA,152848.000,4199.9999,N,00209.9999,E,1,07,1.46,**0010.8**,M,51.3,M,,\*6B


  
La parte del código que se encarga de hacer toda esta adaptación es esta:

```auto
void enviar_comando_gprmc(){
  int i2 = 7;
  //leer y enviar hasta final del comando --> previo al inicio del nuevo $
  Serial.print("$GPRMC,");    //enviamos lo que tenemos en el buffer
  while (bt.peek()!='$'){
    while (bt.available() && bt.peek()!='$'){
      Serial.print((char)bt.read());
    }
  }
}

void enviar_comando_gpgga(){
  int cs;
  int i2;
  leer_comando_entero();
  adaptar_satelites();      //  ",7," --> ",07," i=,
  adaptar_altitud();        //  ",148.0," --> ",00148.0,"
  cs=getCheckSum(buffer);
  Serial.write(buffer);
  Serial.print(cs,HEX);
  Serial.println("");
}

void adaptar_satelites(){
  int n=0;
  int i=0;
  while (n<8)              //llegamos hasta el final del comando numero #8
  {
    i++;
    if (buffer[i]==',') n++;
  }
  if (buffer[i-2]==','){
    for (n=strlen(buffer);n>i-2;n--){
      buffer[n+1] = buffer[n];
    }
    buffer[i-1]='0';
  }
}

void adaptar_altitud(){
  int n=0;
  int i=0;
  int k=0;
  int dig=6;

  while (n<10)   //llegamos hasta el comando n9 -->(k)XXXXXX(i) k=, inicial y i=, final
  {
    i++;
    if (n==8) k=i;
    if (buffer[i]==',') n++;
  }

  if (buffer[k+1]=='-'){
    k++;
    dig--;
  }

  int offset = dig-((i-k)-1);            //la longitud del dígito es de 6 o 5 dependiendo del signo - o +

  for (n=strlen(buffer);n>k;n--){        //desplazamos todo n posiciones segun offset
    buffer[n+offset] = buffer[n];
  }
  for (n=k+1;n<=k+offset;n++) buffer[n]='0';
}
```

Ojo!!! que en ocasiones también pueden venir altitudes negativas. Normalmente cuando no hay suficientes satélites para establecer una posición correcta (el código ya lo contempla).

"*Lo malo*" de modificar la información que se envía de esta sentencia, es que el número de control o *checksum* (para verificar que la info es correcta), no sirve y hay que recalcularlo nuevamente. Para ello hay que utilizar un XOR desde el inicio de la sentencia, "*$*" hasta el "*\**", y luego colocarlo en formato hexadecimal. Esta parte del código no es mía, lo saqué de [aquí](http://timzaman.wordpress.com/code-c-arduino/checksum-xor-cpp/):

```auto
int getCheckSum(char *string) {
  int i2;
  int XOR;
  int c2;
  // Calculate checksum ignoring any $'s in the string
  for (XOR = 0, i2 = 0; i2 < strlen(string); i2++) {
    c2 = (unsigned char)string[i2];
    if (c2 == '*') break;
    if (c2 != '$') XOR ^= c2;
  }
  return XOR;
}
```

Un vez programado, prototipado y probado, ya solo queda traspasarlo y empaquetarlo todo debidamente, ponerle un pulsador de reset exterior y un interruptor de encendido. Para ello voy a utilizar un Arduino mini pro versión de 3.3V a 8Mhz (ya veremos si funciona!!! y es capaz de procesar toda la información correctamente y sin retrasos, ya que sino habrá perdidas en los datos).

**Para acabar [aquí](http://code.google.com/p/arduino-gps-bluetooth-nikond90/downloads/detail?name=btD90.ino&can=2&q= "http://code.google.com/p/arduino-gps-bluetooth-nikond90/downloads/detail?name=btD90.ino&can=2&q=") os dejo el link para descargar el sketch**, está realizado con la versión Arduino 1.0 (tenedlo en cuenta para la compilación). También agradecer en esta parte del proyecto la colaboración de [fefago](https://twitter.com/#!/meinteresatodo) , por su aporte de información y comentarios ;) .

Links de referencia de esta parte:

<http://www.cutedigi.com/pub/Bluetooth/BMX_Bluetooth_quanxin.pdf>

<http://www.gpsinformation.org/dale/nmea.htm>

<http://www.arduino.cc/cgi-bin/yabb2/YaBB.pl?num=1293745670>

<http://timzaman.wordpress.com/code-c-arduino/checksum-xor-cpp/>

<http://air.imag.fr/mediawiki/index.php/Wireless_Bluetooth_RS232_TTL_Transceiver_Module>
