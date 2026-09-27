---
layout: ../../layouts/post.astro
title: "Optimizar Linux a fondo con IA: del diagnóstico al 100% en minutos"
pubDate: 2026-09-26
description: "Cómo usar la IA como copiloto de terminal para auditar, iterar y resucitar un portátil veterano afinando zRAM, TLP y kernel."
author: "akirasan"
isPinned: false
excerpt: "Cómo usar la IA como copiloto de terminal para auditar, iterar y resucitar un portátil veterano afinando zRAM, TLP y kernel."
image:
  src: "/images/2026-09-25-optimizacion-linux-ia-portada.svg"
  alt: "Terminal de Linux con métricas de rendimiento y comandos de optimización"
tags: ["Linux", "IA", "optimización", "zRAM", "sysadmin", "DIY"]
---

<<<<<<< HEAD
Todos tenemos en el taller o por casa algún portátil veterano que empieza a arrastrarse: ventiladores zumbando a tope sin hacer nada, microtirones al abrir dos pestañas del navegador y la sensación de que el hardware ya no da más de sí.

Normalmente, ponerse a afinar un sistema Linux a bajo nivel da pereza: bucear en foros antiguos de hace cinco años, mirar documentación del kernel, probar parámetros de `sysctl` a ciegas y cruzar los dedos para no romper nada. Pero, usando la IA como copiloto técnico interactivo, ese proceso cambia por completo. En lugar de pasar horas buscando información dispersa, vas lanzando comandos de diagnóstico, pegando la salida en crudo y recibiendo un ajuste concreto en cuestión de segundos.
=======
Cómo usar la IA como copiloto de terminal para auditar, iterar y resucitar un portátil veterano afinando zRAM, TLP y kernel.

Todos tenemos en el taller o por casa algún portátil veterano que empieza a arrastrarse: ventiladores zumbando a tope sin hacer nada, microtirones al abrir dos pestañas del navegador y la sensación de que el hardware ya no da más de sí.

Normalmente, ponerse a afinar un sistema Linux a bajo nivel da pereza: bucear en foros antiguos de hace cinco años, mirar documentación del kernel, probar parámetros de `sysctl` a ciegas y cruzar los dedos para no romper nada. No soy sysadmin de formación, así que suelo ir probando sobre la marcha,...pero usando la IA como copiloto técnico interactivo, ese proceso cambia por completo: en vez de pasar horas buscando información dispersa, vas lanzando comandos de diagnóstico, pegando la salida en crudo y recibiendo un ajuste concreto en cuestión de segundos.
>>>>>>> dbfd332 (update tono post 2026)

![](/images/2026-09-25-optimizacion-linux-ia-portada.svg)

Para esta prueba, el conejillo de indias ha sido un **ASUS ZenBook UX330UA** con Linux Mint 22.1 (base Ubuntu 24.04), procesador Intel Core i5-6200U (Skylake) y 8 GB de RAM soldada que no se pueden ampliar.

<<<<<<< HEAD
## Paso 1: radiografía inicial sin rodeos
=======
### Radiografía inicial sin rodeos
>>>>>>> dbfd332 (update tono post 2026)

Lo primero no es inventarse soluciones, sino preguntarle a la máquina qué le pasa. Le pedí a la IA una batería de comandos para sacar una foto completa del estado del hardware, los gobernadores de la CPU, la memoria y los sensores:

```bash
# Diagnóstico completo de componentes y drivers
inxi -Fzxxx

# Servicios de energía activos
systemctl list-units --type=service --state=running | grep -E "tlp|power-profiles|thermal|asus"

# Gobernador de CPU y estado de swap/sensores
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
free -h
swapon --show
sensors
```

En menos de medio minuto, la IA detectó los tres cuellos de botella reales que estaban ahogando el equipo:

<<<<<<< HEAD
1. **Swap colapsada en SSD:** el `/swapfile` de 2 GB estaba al 94,2 % de uso, provocando lecturas y escrituras síncronas continuas en el disco que congelaban la interfaz.
2. **CPU encallada en Turbo:** el procesador no bajaba de 2,8 GHz ni en reposo absoluto, manteniendo el ventilador a más de 4.100 RPM.
3. **Decodificación de vídeo por software:** la GPU integrada carece de soporte por hardware para códecs modernos como AV1 o VP9, comiéndose el 100 % de la CPU al reproducir YouTube.

## Paso 2: adiós a la swap en disco, hola zRAM
=======
* **Swap colapsada en SSD:** el `/swapfile` de 2 GB estaba al 94,2 % de uso, provocando lecturas y escrituras síncronas continuas en el disco que congelaban la interfaz.
* **CPU encallada en Turbo:** el procesador no bajaba de 2,8 GHz ni en reposo absoluto, manteniendo el ventilador a más de 4.100 RPM.
* **Decodificación de vídeo por software:** la GPU integrada carece de soporte por hardware para códecs modernos como AV1 o VP9, comiéndose el 100 % de la CPU al reproducir YouTube.

### Adiós a la swap en disco, hola zRAM
>>>>>>> dbfd332 (update tono post 2026)

El disco SSD SATA no tiene la velocidad de la memoria física. Para evitar que el sistema se ahogue al llenar los 8 GB, configuramos zRAM: un bloque comprimido en la propia RAM física mediante el algoritmo ultrarrápido zstd.

Le pedí a la IA el bloque de comandos para configurarlo al 50 % de la RAM y afinar el comportamiento de paginación del kernel:

```bash
# 1. Instalar utilidades
sudo apt install -y zram-tools

# 2. Asignar el 50% de RAM con compresión zstd
sudo tee /etc/default/zramswap << 'EOF'
ALGO=zstd
PERCENT=50
PRIORITY=100
EOF

# 3. Ajustes de sysctl para priorizar zRAM frente al SSD
sudo tee /etc/sysctl.d/99-memory-tweaks.conf << 'EOF'
vm.swappiness=180
vm.page-cluster=0
vm.watermark_boost_factor=0
vm.watermark_scale_factor=125
EOF

# 4. Aplicar cambios y reactivar la configuración de swap
sudo systemctl restart zramswap
sudo sysctl --system
sudo swapoff /swapfile && sudo swapon /swapfile
```

<<<<<<< HEAD
Al vaciar el archivo del SSD y derivar la carga a zRAM, el compresor empezó a trabajar con una ratio de compresión de aproximadamente 3,4:1. El `/swapfile` en disco pasó al 0 % y los microtirones de la interfaz desaparecieron al instante.

## Paso 3: domesticando frecuencias y energía con TLP
=======
A la primera pasada me quedé corto con el `PERCENT` y el sistema seguía tirando de disco en cuanto abría un par de máquinas virtuales de más,...así que tocó subirlo. Con el 50 % ya bien aplicado, el compresor empezó a trabajar con una ratio de compresión de aproximadamente 3,4:1. El `/swapfile` en disco pasó al 0 % y los microtirones de la interfaz desaparecieron al instante. Menudo cambio!!!

### Domesticando frecuencias y energía con TLP
>>>>>>> dbfd332 (update tono post 2026)

Linux Mint trae por defecto `power-profiles-daemon`, que en arquitecturas Intel de sexta generación no afina bien los estados de `intel_pstate`. La IA me generó la receta para sustituirlo por TLP y configurar un perfil equilibrado:

```bash
# Desactivar el demonio por defecto e instalar TLP
sudo apt install -y tlp tlp-rdw
sudo systemctl stop power-profiles-daemon
sudo systemctl mask power-profiles-daemon
sudo systemctl enable --now tlp

# Crear un perfil a medida para Intel Skylake
sudo tee /etc/tlp.d/01-skylake-cpu.conf << 'EOF'
CPU_SCALING_GOVERNOR_ON_AC=powersave
CPU_SCALING_GOVERNOR_ON_BAT=powersave
CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power
CPU_MIN_PERF_ON_AC=0
CPU_MAX_PERF_ON_AC=100
CPU_MIN_PERF_ON_BAT=0
CPU_MAX_PERF_ON_BAT=75
EOF
sudo tlp start
```

<<<<<<< HEAD
**Resultado inmediato:** la frecuencia de los núcleos en reposo cayó de los 2,8 GHz fijos a 1,2 GHz, reduciendo el consumo basal de la CPU a solo 2–3 W.

## Paso 4: aligerar YouTube y los gráficos
=======
La frecuencia de los núcleos en reposo cayó de los 2,8 GHz fijos a 1,2 GHz, reduciendo el consumo basal de la CPU a solo 2-3 W. El ventilador por fin calla ;)

### Aligerar YouTube y los gráficos
>>>>>>> dbfd332 (update tono post 2026)

Para evitar que la CPU se fría reproduciendo vídeo en el navegador, forzamos la aceleración por hardware con drivers VA-API y habilitamos la compresión de framebuffer en el kernel:

```auto
# Instalar aceleración gráfica VA-API
sudo apt install -y intel-media-va-driver-non-free vainfo i965-va-driver-shaders

# Habilitar compresión de framebuffer (FBC) para rascar 1 W de consumo
echo "options i915 enable_fbc=1 enable_psr=1" | sudo tee /etc/modprobe.d/i915.conf
sudo update-initramfs -u
```

<<<<<<< HEAD
Y el toque clave en el navegador: instalar la extensión `enhanced-h264ify` para bloquear los códecs AV1 y VP9. Al forzar a YouTube a enviar H.264 —que la gráfica decodifica de forma nativa—, el uso de procesador viendo vídeo a 1080p se desplomó.
=======
Y el toque clave en el navegador: instalar la extensión `enhanced-h264ify` para bloquear los códecs AV1 y VP9. Al forzar a YouTube a enviar H.264, que la gráfica decodifica de forma nativa, el uso de procesador viendo vídeo a 1080p se desplomó.
>>>>>>> dbfd332 (update tono post 2026)

La gracia está en no limitarse a pedir respuestas genéricas: si interpretas la salida real de tu terminal (`inxi`, `sensors`, `swapon`), la IA puede devolverte ficheros de configuración listos para aplicar con `sudo tee`. En mi caso, eso me ha ahorrado tardes enteras de pruebas y errores ;)
