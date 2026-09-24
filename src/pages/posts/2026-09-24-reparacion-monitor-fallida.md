---
layout: ../../layouts/post.astro
title: "Reparando la retroiluminación de un monitor (o cómo morir en el intento)"
pubDate: 2026-09-24
description: "Desmontando un monitor PcCom para reparar su tira LED de retroiluminación. Una odisea de capas, voltajes y un final inesperado."
author: "akirasan"
isPinned: false
excerpt: "Desmontando un monitor PcCom para reparar su tira LED de retroiluminación. Una odisea de capas, voltajes y un final inesperado."
image:
  src: "/images/2026-09-24-reparacion-monitor-09.webp"
  alt: "Monitor PcCom funcionando con fallo en la zona derecha"
tags: ["hardware", "DIY", "reparacion", "teardown", "maker"]
---

Tenía por aquí este monitor PcCom de 27" que de repente decidió dejar a oscuras media pantalla. Se encendía, daba señal, pero toda la parte derecha se quedaba en penumbra. Clásico fallo de la retroiluminación (backlight).

![](/images/2026-09-24-reparacion-monitor-01.webp)

Desmontar un panel LCD siempre tiene su miga: el cristal es fino como el papel, las capas difusoras van milimétricamente encajadas y cualquier mota de polvo o presión de más te arruina la pantalla. Pero como aquí no tiramos nada sin intentar arreglarlo antes, tocó meterlo a quirófano.

### Destripando el monitor

Abrir la carcasa trasera no tiene mucho misterio: tornillos de rigor, pestañas de plástico con cuidado de no partirlas y fuera la tapa.

Dentro nos encontramos la electrónica básica: la placa controladora (mainboard), la fuente interna/inverter y los cables flexibles (flat cables) que van hacia las placas del panel LCD (T-Con integrada).

![](/images/2026-09-24-reparacion-monitor-02.webp)

![](/images/2026-09-24-reparacion-monitor-03.webp)

Mucho ojo con esas tiras de flex amarillas que van unidas al cristal: van pegadas por calor y si desgarras una, adiós monitor para siempre. Toca levantarlas con mimo extremo para poder liberar el chasis metálico.

![](/images/2026-09-24-reparacion-monitor-04.webp)

### Llegando a la tira de LEDs

Una vez retirado el panel LCD y las láminas polarizadas y difusoras de luz, llegamos al fondo del asunto: la placa acrílica guía de luz y el perfil metálico donde va alojada la tira LED en el borde inferior (edge-LED).

![](/images/2026-09-24-reparacion-monitor-05.webp)

![](/images/2026-09-24-reparacion-monitor-06.webp)

La tira viene pegada con cinta térmica de doble cara al propio disipador de aluminio. Al meterle corriente con la fuente para probarla... ¡bingo! La mitad izquierda encendía con buena intensidad, pero la mitad derecha estaba completamente muerta. Varios LEDs en serie habían pasado a mejor vida, cortando el circuito.

![](/images/2026-09-24-reparacion-monitor-07.webp)

### La rotura en el peor momento: FALLO!!!

Aquí vino el drama. Tras localizar los diodos dañados e intentar sanear la tira, en pleno proceso de manipulación y reensamblado forcé ligeramente la zona debilitada y... adiós, me cargué físicamente varios LEDs más de la pista.

![](/images/2026-09-24-reparacion-monitor-08.webp)

Un desastre. La tira quedó inservible y sin opción de parche rápido con el soldador. 

Aún así monté de nuevo todo el sándwich de capas y el marco para verificar que al menos el panel de cristal LCD no había sufrido durante el desmontaje.

![](/images/2026-09-24-reparacion-monitor-09.webp)

Como veis, el panel está vivo y la imagen se ve de lujo en la parte izquierda, pero la mitad derecha sigue en tinieblas esperando luz. 

### ¿Y ahora qué?

Una lástima haber tenido todo desmontado y listo para resolverlo y tropezar al final, pero de todo se aprende. Al menos tengo la referencia exacta de la tira original (longitud, voltaje por LED y disposición del conector), así que ya estoy rastreando repuesto para cambiar la tira completa por una nueva en lugar de andar puenteando diodos sueltos.

En cuanto me llegue la tira de recambio, segunda parte del asalto ;)