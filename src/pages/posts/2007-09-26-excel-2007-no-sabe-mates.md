---
layout: ../../layouts/post.astro
title: "Excel 2007 no sabe mates"
pubDate: 2007-09-26
description: "Al parecer se  en la última versión del Office 2007, en el producto Excel. Por lo visto (no he tenido ocasión de comprobarlo) al realizar la"
author: "akirasan"
isPinned: false
excerpt: "Al parecer se  en la última versión del Office 2007, en el producto Excel. Por lo visto (no he tenido ocasión de comprobarlo) al realizar la"
image:
  src: ""
  alt: "Excel 2007 no sabe mates"
tags: []
---

Al parecer se [ha detectado un bug](http://groups.google.com/group/microsoft.public.excel/browse_thread/thread/2bcad1a1a4861879/2f8806d5400dfe22?hl=en#2f8806d5400dfe22 "http://groups.google.com/group/microsoft.public.excel/browse_thread/thread/2bcad1a1a4861879/2f8806d5400dfe22?hl=en#2f8806d5400dfe22") en la última versión del Office 2007, en el producto Excel. Por lo visto (no he tenido ocasión de comprobarlo) al realizar la siguiente operación ***850 x 77,1*** devuelve **100000**,...madre mía!!! (resultado correcto: 65535).

**Actualizado:** En realidad el bug se produce con cualquier operación que de como resultado 65535 (gracias Félix por el aviso). Por lo visto es un tema de visualización e internamente el resultado se guarda correctamente en la celda.
