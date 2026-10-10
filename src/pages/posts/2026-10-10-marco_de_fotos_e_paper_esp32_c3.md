---
layout: ../../layouts/post.astro
title: "Marco de fotos de tinta electrónica con ESP32-C3 y WebApp móvil"
pubDate: 2026-10-10
description: "Cómo montar un marco de fotos minimalista con pantalla e-Paper de 1,54″ y subir fotos desde el móvil sin instalar apps de terceros."
author: "akirasan"
isPinned: false
excerpt: "Cómo montar un marco de fotos minimalista con pantalla e-Paper de 1,54″ y subir fotos desde el móvil sin instalar apps de terceros."
image:
  src: "/images/2026-10-10-esp32c3_fotomarco-01.webp"
  alt: "Marco de fotos digital con pantalla e-Paper y ESP32-C3"
tags: ["ESP32", "e-Paper", "hardware", "DIY", "maker", "PlatformIO"]
---

Tenía ganas de cacharrear con una pantalla de tinta electrónica pequeña que tenía por el cajón y hacerme un marco de fotos para la mesa de trabajo. La idea de partida era sencilla pero tenía que ser cómoda de usar: nada de conectar el microcontrolador al ordenador cada vez que quisiera cambiar la foto, ni de tener que instalar aplicaciones raras o compilar APKs en el móvil.

La solución ha sido montar un sistema autónomo: el propio microcontrolador genera su propia red Wi-Fi con portal cautivo, te conectas desde el teléfono, se abre una interfaz web en el navegador, eliges cualquier foto de tu galería, la encuadras y la mandas a la pantalla en un par de clics.


## ¿Qué necesitamos?

Para el montaje he optado por componentes muy económicos y fáciles de conseguir:

* **ESP32-C3 SuperMini:** Una placa diminuta basada en arquitectura RISC-V con conectividad Wi-Fi y puerto USB-C nativo.
* **Pantalla E-Paper WeAct Studio 1.54″:** Panel monocromo (blanco y negro) de 200×200 píxeles con controlador SSD1681.
* **Cables de conexión** y un cable USB-C estándar para alimentarlo.

## Conexión del hardware

El conexionado entre el módulo e-Paper de WeAct y la placa ESP32-C3 SuperMini va por bus SPI. Son ocho pines directos:

| Pin Pantalla WeAct | Pin ESP32-C3 | Función |
| :--- | :--- | :--- |
| **BUSY** | **GPIO 5** | Estado del refresco |
| **RES** | **GPIO 2** | Reset del panel |
| **D/C** | **GPIO 3** | Selección Dato / Comando |
| **CS** | **GPIO 7** | Chip Select |
| **SCL** | **GPIO 4** | Reloj SPI (SCK) |
| **SDA** | **GPIO 6** | Datos SPI (MOSI) |
| **GND** | **GND** | Masa |
| **VCC** | **3.3V** | Alimentación lógica (3.3V) |

**Ojo:** la pantalla funciona estrictamente a 3.3V. Asegúrate de alimentar el pin VCC desde la salida de 3.3V del ESP32-C3 y no desde los 5V directos del USB.

## La arquitectura: procesar en el móvil, no en el microcontrolador

El primer dilema técnico fue cómo gestionar las imágenes. Convertir un JPEG o PNG pesado de un móvil moderno a un bitmap monocromo de 200×200 píxeles con tramado requiere bastante cálculo y memoria, algo justo para un microcontrolador si además está atendiendo peticiones web.

La gracia del proyecto está en delegar todo ese trabajo al navegador del teléfono:

1. El ESP32-C3 levanta un punto de acceso abierto llamado `Marco-Fotos-AP`.
2. Gracias a un servidor DNS interno (`DNSServer`), al conectarte salta automáticamente el portal cautivo en iOS y Android sin tener que escribir `192.168.4.1` a mano.
3. El navegador carga una *Single Page Application* embebida en la memoria Flash del ESP32 con diseño táctil estilo iOS.

![Interfaz web móvil para encuadrar y tramado](/images/2026-10-10-esp32c3_fotomarco-webapp_interfaz.png)

Desde la propia interfaz web puedes mover la imagen con el dedo, hacer zoom, rotarla, ajustar brillo y contraste, y seleccionar el algoritmo de tramado (*dithering*) en tiempo real sobre un elemento `<canvas>`:

* **Floyd-Steinberg (Modo Foto):** Gradaciones suaves, ideal para fotos normales y escenas complejas.
* **Atkinson (Modo Nítido):** Mayor contraste y definición de bordes, fantástico para retratos.
* **Umbral B/N:** Blanco y negro puro sin puntos intermedios, perfecto para logos, ilustraciones o texto.

Una vez encuadrada, el JavaScript del navegador empaqueta la imagen en un `Blob` binario de exactamente 5.000 bytes (200×200 bits a 1 bit por píxel) y lo envía mediante una petición POST multipart al ESP32. El microcontrolador solo tiene que recibir los 5 KB por fragmentos y volcarlos directamente al búfer de la pantalla con la librería `GxEPD2`.

## El tropiezo de la polaridad

Durante las primeras pruebas todo parecía funcionar a la perfección en la web, pero al pintar la imagen en la tinta electrónica salía en negativo: las zonas oscuras quedaban blancas y viceversa. 

El fallo estaba en la llamada a la librería gráfica: inicialmente usé `display.drawInvertedBitmap()`, pensando en la polaridad habitual de las pantallas OLED. En este panel de tinta electrónica, al mandar el bit `1` como negro desde el cliente, la función invertía el valor y dejaba el resultado al revés. Bastó cambiar a `display.drawBitmap()` para que los contrastes quedasen clavados.

## Código y puesta en marcha

Todo el proyecto está estructurado para compilar con **PlatformIO** sobre el framework de Arduino. En el archivo `platformio.ini` solo necesitamos definir la placa y las dos dependencias clave:

```ini
[env:esp32-c3-supermini]
platform = espressif32
board = esp32-c3-devkitm-1
framework = arduino
monitor_speed = 115200
build_flags = 
    -D ARDUINO_USB_MODE=1
    -D ARDUINO_USB_CDC_ON_BOOT=1
lib_deps = 
    zinggjm/GxEPD2 @ ^1.5.8
    adafruit/Adafruit GFX Library @ ^1.11.9
```

Lo mejor de la tecnología e-Paper es que una vez que la pantalla termina el ciclo de refresco y entra en modo hibernación, la imagen se queda fijada de forma permanente en el panel incluso aunque desconectes el cable de alimentación.

El proyecto es completamente funcional y está pensado para que cualquiera pueda montárselo en una tarde. Lo siguiente será diseñar una pequeña carcasa en 3D para dejarlo curioso en el escritorio.

**Código fuente en GitHub:** <https://github.com/akirasan/esp32-c3_fotomarco>