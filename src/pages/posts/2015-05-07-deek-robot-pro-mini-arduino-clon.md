---
layout: ../../layouts/post.astro
title: "Deek-Robot Pro Mini (arduino clon)"
pubDate: 2015-05-07
description: "He adquirido recientemente unos cuantos de estos clones de Arduino Pro Mini con la idea de programar futuros sensores para el Internet de la"
author: "akirasan"
isPinned: false
excerpt: "He adquirido recientemente unos cuantos de estos clones de Arduino Pro Mini con la idea de programar futuros sensores para el Internet de la"
image:
  src: "/content/images/2015/05/deekrobot_pins.JPG"
  alt: "Deek-Robot Pro Mini (arduino clon)"
tags: ["Arduino"]
---

[Deek-Robot Pro Mini](http://arduino-board.com/boards/dr-pro-mini) He adquirido recientemente unos cuantos de estos clones de Arduino Pro Mini con la idea de programar futuros sensores para el Internet de las Cosas. Son muy baratos, unos 2,15€/unidad con envío desde China incluido.

Al ser unos módulos Pro Mini no llevan un conector USB para poder programarlos, por lo que hay que soldarle unas patillas de conexión y utilizar un módulo FTDI por USB.

![arduino_deek_robot_pro_mini_pines_FTDI](/content/images/2015/05/deekrobot_pins.JPG)

### Características técnicas

Estas son las características técnicas de este clon de Arduino:

**Deek-Robot Pro Mini**

|  |  |
| --- | --- |
| MCU | ATmega328 |
| DigitalPins | 14 |
| PWM | 6 |
| Analog Inputs | 8 |
| Analog Outputs | 0 |
| Operating Voltage | 5v |
| Operating Frequency | 16MHz |
| 3.3V Output | None |
| Test Current Draw | 18mA |
| Input Voltage | 3v - 12v |

### Conexiones del Deek-Robot Pro Mini al FTDI

Incluyo este apartado porque me ha costado un poco encontrar información de como programar este tipo de arduino, así que pongo la tabla de conexión utilizando un FTDI USB para que nadie tenga dudas, aunque veréis que es muy sencillo, la duda principal era que hacíamos con el pin CTS del FTDI,...pues nada :)

| PIN DEEK-ROBOT | PIN FTDI |
| --- | --- |
| DTR | DTR |
| TXD | RXD |
| RXD | TXD |
| VCC | VCC |
| GND | GND |
| GND |  |
|  | CTS |

![arduino_deek_robot_pro_mini_pines_FTDI](/content/images/2015/05/deekrobot_pins_ftdi-1.JPG)

A la hora de programarlo con el IDE de Arduino, simplemente hay que decirle que es una placa *Arduino Pro Mini* y recordad que trabaja con un ATmega328 a 5v y 16Mhz

![arduino_deek_robot_IDE](/content/images/2015/05/deekrobot_IDE_arduino.jpg)

### ¿Dónde comprarlo?

Yo lo he comprado a muy buen precio en [Banggood](http://www.banggood.com/5Pcs-Arduino-5V-16M-Pro-Mini-Microcontroller-Improved-AtMega328P-p-951795.html). Evidentemente por eBay seguro que también se pueden encontrar e incluso parecidos. O también en su website [Deek-Robot](http://www.deek-robot.com/Shopping.asp)
