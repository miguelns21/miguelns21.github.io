---
layout: single
title: Intranet/Nextcloud (Parte 3/4)
excerpt: "Nuestro segundo y último script “repartir_ficheros.py” tiene como misión repartir los ficheros que el administrador suba a la carpeta “Reparto_Factura” de su zona de administración a todos los usuarios del sistema que les correspondan."
date: 2021-08-16
classes: wide
header:
  teaser: /assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-17.42.27.png
  teaser_home_page: true
categories:
  - Script
tags:
  - Intranet
  - Nextcloud
---

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/nextcloud-square-logo.png">
</p>

# Repartir Facturas

Nuestro segundo y último script “repartir_ficheros.py” tiene como misión repartir los ficheros que el administrador suba a la carpeta “Reparto_Factura” de su zona de administración a todos los usuarios del sistema que les correspondan.

La lógica debe ser que el nombre del fichero a repartir comience con el código del usuario en nuestra intranet.

Por ejemplo: la factura “1000-noviembre.pdf” moverá dicho fichero a la zona de usuario con código 1000.

El administrador puede crear una estructura de carpetas en su carpeta “Reparto_Factura“ que se replicará a la zona del usuario. Por ejemplo:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-17.42.27.png">
</p>

## Cron

Al igual que el script de creación de usuario, nuestro script de reparto se debe programar con la frecuencia estimada por el administrador de la plataforma atendiendo a sus necesidades. Esto es, si las facturas se suben con poca frecuencia, no tiene sentido establecer la programación muy frecuentemente.

En el caso de Hostens, el panel de control nos ofrece un asistente para los trabajos de cron como se muestra a continuación:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-17.48.30.png">
</p>

Fichero de creación de usuarios programado cada 5 minutos.
