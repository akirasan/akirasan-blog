---
layout: ../../layouts/post.astro
title: "Cómo añadir iconos de redes sociales en un tema del blog Ghost"
pubDate: 2016-05-13
description: "Uno de los temas que más me gusta para  (y es el que estoy utilizando actualmente) es el de . Pero uno de los problemas que tiene es que no"
author: "akirasan"
isPinned: false
excerpt: "Uno de los temas que más me gusta para  (y es el que estoy utilizando actualmente) es el de . Pero uno de los problemas que tiene es que no"
image:
  src: "/content/images/2016/05/Selecci-n_063.jpg"
  alt: "Cómo añadir iconos de redes sociales en un tema del blog Ghost"
tags: ["blog", "Ghost"]
---

Uno de los temas que más me gusta para [Ghost](https://ghost.org/es/) (y es el que estoy utilizando actualmente) es el de [Saga](http://saga.gustavlindqvist.se/). Pero uno de los problemas que tiene es que no tiene iconos para compartir posts en las redes sociales. Algo que no entiendo porque no está incluido.

Bueno, la cosa es que es bien sencillo, pero hay que tocar el código con el que está hecho el tema. Pero son modificaciones muy sencillas.

Lo primero que tenemos que hacer es editar el fichero `post.hbs` que normalmente lo vamos a encontrar en el path dónde tenemos instalado ghost `<path-ghost>/content/themes/saga/`, e insertar este código:

```auto
<span id="shared" align="center">
  <h4>Compartir</h4> 
  <div>
  <a style="padding-right:15px;padding-left:15px" class="fa fa-twitter fa-3x" href="https://twitter.com/intent/tweet?text={{encode title}}&amp;url={{url absolute="true"}}&amp;via=akirasan" onclick="window.open(this.href, 'twitter-share', 'width=550,height=235');return false;">
   <span class="hidden">Twitter</span>
   </a>
   <a style="padding-right:15px;padding-left:15px" class="fa fa-facebook fa-3x" href="https://www.facebook.com/sharer/sharer.php?u={{url absolute="true"}}" onclick="window.open(this.href, 'facebook-share','width=580,height=296');return false;">
    <span class="hidden">Facebook</span>
    </a>
    <a style="padding-right:15px;padding-left:15px" class="fa fa-google-plus fa-3x" href="https://plus.google.com/share?url={{url absolute="true"}}" onclick="window.open(this.href, 'google-plus-share', 'width=490,height=530');return false;">
     <span class="hidden">Google+</span>
     </a>
  </div>
</span>
```

Vamos por parte, cada icono de red social está compuesto por los mismos componentes que se definen en `<a>..</a>` y los iconos se definen así `class="fa fa-twitter fa-3x"`. Son iconos de la [Font Awesom Icons](http://fontawesome.io/icons/) y que podéis modificar y/o adaptar a un tamaño mas grande o pequeño:

![](/content/images/2016/05/Selecci-n_064.jpg)

Ah!! un detallito, para Twitter en la URL se pasa un parámetro `via=akirasan` que sirve para que en el texto del tweet aparezca la mención de tu cuenta.

**¿Pero dónde?**, pues justo es esta parte del código, justo después del tag `{{content}}` y antes del `</section>`, por lo que quedará dentro de lo que sería el post en:

```auto
<main id="main" class="content">
    <article itemtype="http://schema.org/BlogPosting" class="animated fadeIn content post {{post_class}}">
     <section class="post-content">
            {{content}}
     </section>
    </article>
</main>
```

Y el resultado final es un apartado de compartir en redes sociales al final de cada post:
