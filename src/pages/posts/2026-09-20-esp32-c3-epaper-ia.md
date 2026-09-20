---
layout: ../../layouts/post.astro
title: "Prototipado rápido con IA: ESP32-C3, pantalla e-Paper y diseño de interfaz"
pubDate: 2026-09-20
description: "De una foto y un prompt en lenguaje natural a tener una estación meteo con diseño limpio tipo Apple y portal cautivo funcionando."
author: "akirasan"
isPinned: false
excerpt: "De una foto y un prompt en lenguaje natural a tener una estación meteo con diseño limpio tipo Apple y portal cautivo funcionando."
image:
  src: "/images/IMG_20260915_215055617.webp"
  alt: "Estación meteorológica en pantalla e-paper con ESP32-C3"
tags: ["ESP32-C3", "e-Paper", "IA", "DIY", "maker", "PlatformIO"]
---

Lo reconozco: cada vez que tengo una pantalla nueva en las manos, ponerme a calcular píxeles a mano, buscar qué driver exacto lleva y pelearme con el diseño de la interfaz me da una pereza tremenda.

Hacer que una pantalla muestre datos es fácil; hacer que **se vea bonita, con una buena experiencia de usuario (UX) y proporciones cuidadas**, suele llevar horas de probar, ajustar coordenadas, recompilar y volver a medir. Por eso quise hacer un experimento radical: ver hasta dónde podía llegar con un modelo de IA multimodal partiendo simplemente de una foto de los componentes encima de la mesa, sin darle modelos exactos ni buscar esquemas de pines.

![](/images/IMG_20260915_211612673.webp)

### El primer prompt: cero especificaciones

Puse la placa y la pantalla juntas, saqué la foto con el móvil y le solté esto a la IA tal cual, sin más datos:

> *"Serías capaz de generar un código para hacer un 'hola mundo', con estos componentes. Lo quiero hacer con vs Code y platform IO, que te parece? O algo más sencillo? La programación tipo Arduino me está bien."*

Solo con la imagen identificó los dos cacharros al vuelo: una placa **ESP32-C3 SuperMini** y un módulo **WeAct Studio E-Paper de 1.54" (200x200 px)** con bus SPI. Pero lo mejor no fue eso, sino que me devolvió la tabla completa de conexiones físicas con los GPIOs estándar, la configuración lista para el `platformio.ini` y el código con la librería `GxEPD2`. 

Conectar los cables según su tabla, compilar en PlatformIO y... ¡a la primera!

![](/images/IMG_20260915_213746821.webp)

### Saltando a bitmaps con dithering

Una vez validado el panel, quise probar imágenes reales. El reto aquí es que estas pantallas son estrictamente blanco y negro (1 bit), por lo que si metes una foto normal sin procesar te sale una masa de manchas negras.

Le pedí mostrar una foto mía, y en lugar de limitarse a decirme "usa un conversor", me detalló el flujo completo: recorte a proporción 1:1, uso de la web `image2cpp` activando **dithering con algoritmo Floyd-Steinberg** (para simular sombras y medios tonos mediante patrones de puntos) y cómo encapsular el array de bytes en un archivo `imagen.h` usando `PROGMEM`.

![](/images/IMG_20260915_215055617.webp)

Un detalle divertido: al principio me salieron los colores invertidos (la barba blanca y el cielo negro), pero en lugar de volver a generar todo el array de bytes, la IA me dio el atajo en código: cambiar la función `display.drawBitmap` por `display.drawInvertedBitmap`. Asunto resuelto.

### El verdadero ahorro de tiempo: UX, iconos matemáticos y portal cautivo

Aquí es donde está la chicha del proyecto y donde realmente se nota el salto de productividad. Quería convertir el cacharro en una **estación meteorológica conectada**, pero con dos requisitos clave que normalmente te comen horas de desarrollo:

1. **Cero credenciales fijas:** Si cambio de red o me llevo el cacharro, no quiero tener que recompilar el firmware. Necesitaba un portal cautivo donde conectarme con el móvil y configurar la WiFi.
2. **Estética cuidada:** Nada del típico texto tosco con fuentes de 8 bits y cajas gruesas. Le pedí un diseño minimalista y limpio, al más puro estilo de los widgets de Apple / iOS.

#### 1. Generación de iconos vectoriales por código

Cualquiera que haya diseñado interfaces para microcontroladores sabe el suplicio que es: o cargas bitmaps pesados para cada estado del tiempo o te toca calcular a mano círculos, líneas y trigonometría para pintar las cosas. 

La IA generó directamente funciones vectoriales para los iconos usando primitivas de dibujo (`fillCircle`, `drawLine`, `fillRoundRect`) con cálculo de ángulos para los rayos de sol, halos de recorte para simular profundidad entre nubes y gotas de lluvia estilizadas. Un ejemplo de cómo resuelve el sol y la nube:

![](/images/IMG_20260916_202450860.webp)


```cpp
void drawAppleSunCloud(int cx, int cy) {
    // Sol de fondo
    display.fillCircle(cx + 9, cy - 8, 8, GxEPD_BLACK);
    display.drawLine(cx + 9, cy - 19, cx + 9, cy - 16, GxEPD_BLACK);
    display.drawLine(cx + 19, cy - 8, cx + 16, cy - 8, GxEPD_BLACK);
    display.drawLine(cx + 16, cy - 15, cx + 14, cy - 13, GxEPD_BLACK);

    // Halo blanco de recorte (estilo SF Symbols)
    display.fillRoundRect(cx - 20, cy, 40, 16, 8, GxEPD_WHITE);
    display.fillCircle(cx - 7, cy + 2, 11, GxEPD_WHITE);
    display.fillCircle(cx + 6, cy - 1, 14, GxEPD_WHITE);

    // Nube en primer plano
    drawAppleCloud(cx, cy);
}


