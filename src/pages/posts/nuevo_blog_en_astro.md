---
layout: ../../layouts/post.astro
title: "Nuevo dominio, nuevo motor: de Ghost a Astro en akirasan.xyz"
pubDate: 2024-05-15
description: "Digo adiós a akirasan.net y a Ghost para renacer en akirasan.xyz con Astro, GitHub y Cloudflare Pages. Un stack moderno, ligero y 100% en modo maker."
author: "akirasan"
isPinned: false
excerpt: "Renovarse o morir: cómo he migrado mi viejo blog en Ghost a un generador estático con Astro, alojado gratis en Cloudflare Pages y bajo el nuevo dominio akirasan.xyz."
image:
  src: "/images/newblogastro.jpg"
  alt: "Comparativa visual entre el antiguo blog en Ghost y el nuevo en Astro"
tags: ["Astro", "Blog", "Cloudflare", "GitHub", "Maker", "Web"]
---

Dicen que no hay mal que por bien no venga, y en este caso el refrán se ha cumplido al pie de la letra. 

Tras perder el dominio histórico **akirasan.net**, me encontré ante la típica encrucijada: lamentarme o aprovechar el tropiezo para hacer borrón y cuenta nueva. Decidí lo segundo: estrenar casa en **[akirasan.xyz](https://akirasan.xyz)** y, de paso, mandar al desguace todo el tinglado técnico anterior para montar algo mucho más ágil, minimalista y divertido de mantener.

![Comparativa del blog antiguo en Ghost vs el nuevo en Astro](/images/newblogastro.jpg)

---

## El punto de partida: adiós a Ghost y al servidor local

Durante bastante tiempo estuve usando **Ghost**. Es un CMS increíble para escribir: limpio, con un editor markdown nativo impecable y un diseño por defecto muy cuidado. 

Sin embargo, en mi configuración tenía varias pegas:
- **Mantenimiento del servidor:** Lo corría en local/homelab, lo que implicaba vigilar túneles, actualizaciones de base de datos (Node, MySQL/SQLite), backups y certificados SSL.
- **Sobrecarga de recursos:** Para un blog personal donde prima el contenido técnico y las notas de taller, mantener un backend dinámico levantado 24/7 consume más energía y tiempo de administración de lo que realmente aporta.
- **Pérdida del dominio:** Con la pérdida de `.net`, tocaba redirigir, tocar configuraciones del CMS y rehacer rutas.

Era el momento idóneo para dar el salto al paradigma de los **sitios estáticos modernos (Jamstack)**.

---

## El nuevo stack: Astro + GitHub + Cloudflare Pages

El objetivo era claro: **cero costes de mantenimiento, velocidad instantánea y control total de cada línea de código**.

```
[Markdown local / VS Code] 
          │
          ▼  git push
    [GitHub Repo]
          │
          ▼  Webhook de despliegue
[Cloudflare Pages + Build de Astro] ──► akirasan.xyz (Edge CDN)
```

### 1. El motor: Astro 🚀
Elegí **[Astro](https://astro.build/)** porque está pensado exactamente para esto: sitios web orientados a contenido. 
- Genera **HTML puro por defecto** (cero JavaScript innecesario en el cliente).
- Lee directamente archivos Markdown o MDX organizados en carpetas gracias a sus *Content Collections*.
- Permite usar componentes si el día de mañana me apetece meter interactividad en un post (con React, Svelte o componentes nativos de Astro).

Cogí un theme base minimalista de estética terminal / código y lo he adaptado por completo a mis necesidades: tipografías de ancho fijo, iconos limpios, etiquetas bien organizadas y paleta oscura/clara.

### 2. El repositorio: GitHub 🐙
Todo el blog vive en un repositorio privado en GitHub. Escribir un post ahora es tan sencillo como abrir Neovim o VS Code, crear un fichero `.md`, añadir las fotos en la carpeta correspondiente y lanzar un commit:

```bash
git add .
git commit -m "feat: nuevo post migración a Astro"
git push origin main
```

### 3. El hosting y DNS: Cloudflare Pages ☁️
La gestión de dominio y el despliegue automático corren a cargo de **Cloudflare**:
- Compré y configuré las DNS del nuevo dominio **akirasan.xyz** directamente en su panel.
- Vinculé el repositorio con **Cloudflare Pages**. Cada vez que hago un `push` a la rama principal, Cloudflare ejecuta el comando `npm run build`, compila el sitio en segundos y lo distribuye por su red global de servidores CDN.
- Incluye SSL/HTTPS automático, caché ultrarrápida y protección sin tocar una sola línea de configuración de servidor web tipo Nginx o Apache.

---

## ¿Qué vas a encontrar a partir de ahora por aquí?

El cambio de envoltorio no es solo estético; marca el inicio de una etapa mucho más enfocada al **mundo maker y cacharreo**:

* **Diseño y prototipado de PCBs:** proyectos con KiCad, microcontroladores ESP32/ESP8266 y placas auxiliares.
* **Impresión 3D:** calibraciones, mejoras mecánicas, modelado funcional y ajustes en Klipper/Marlin.
* **Domótica y Home Assistant:** integración de sensores MQTT, automatizaciones reales y scripts en Linux.
* **Notas de taller:** pequeños trucos de terminal, código rápido y soluciones a problemas cotidianos para que puedas replicarlos sin comerte la cabeza.

Si vienes del blog antiguo, ¡bienvenido a la nueva guarida! Ponte cómodo, revisa los tags y prepárate para mancharte las manos con código y estaño.