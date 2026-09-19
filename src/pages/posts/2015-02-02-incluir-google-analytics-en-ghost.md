---
layout: ../../layouts/post.astro
title: "Incluir Google Analytics en Ghost"
pubDate: 2015-02-02
description: "Si queremo medir el tráfico que llega a nuestro blog creado en Ghost mediante , debemos incluir el tag en el siguiente sitio (justo antes de"
author: "akirasan"
isPinned: false
excerpt: "Si queremo medir el tráfico que llega a nuestro blog creado en Ghost mediante , debemos incluir el tag en el siguiente sitio (justo antes de"
image:
  src: ""
  alt: "Incluir Google Analytics en Ghost"
tags: ["Ghost"]
---

Si queremo medir el tráfico que llega a nuestro blog creado en Ghost mediante [Google Analytics](http://www.google.es/analytics/), debemos incluir el tag en el siguiente sitio (justo antes del final del tag html `</HEAD>`, dentro del fichero `<path_ghost>/content/themes/casper/default.hbs` (tema utilizado [Casper](http://allghostthemes.com/casper/)):

**Actualizado 08/05/2015:** Desde la versión 6.0.2 en los Settings se ha habilitado la opción **Code Injection** que ya puedes utilizar para incluir este tipo de código.

```auto
<!DOCTYPE html>
<html>
<head>
{{! Document Settings }}
<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />

{{! Page Meta }}
<title>{{meta_title}}</title>
<meta name="description" content="{{meta_description}}" />

<meta name="HandheldFriendly" content="True" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<link rel="shortcut icon" href="{{asset "favicon.ico"}}">

{{! Styles'n'Scripts }}
<link rel="stylesheet" type="text/css" href="{{asset "css/screen.css"}}" />
<link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Merriweather:300,700,700italic,300i$

{{! Ghost outputs important style and meta data with this tag }}
{{ghost_head}}

<!-- Google Analytics -->
<!-------- AQUI TU CÓDIGO GOOGLE ANALYTICS ------->
<!-- Google Analytics FIN -->

</head>

<body class="{{body_class}}">
```
