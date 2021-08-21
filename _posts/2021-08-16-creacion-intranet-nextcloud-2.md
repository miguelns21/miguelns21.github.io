---
layout: single
title: Intranet/Nextcloud (Parte 2/4)
excerpt: "Nuestro primer script “importar_usuario.py” tiene como misión importar los ficheros CSV que el administrador suba a la carpeta Clientes_CSV de nuestra intranet."
date: 2021-08-16
classes: wide
header:
  teaser: /assets/images/creacion-intranet-nextcloud/nextcloud-square-logo.png
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

# Importar Usuarios

Nuestro primer script “importar_usuario.py” tiene como misión importar los ficheros CSV que el administrador suba a la carpeta Clientes_CSV de nuestra intranet. Los pasos que sigue en script son:

1. Crea el usuario en NextCloud.
2. Manda un email a dicho usuario notificándole su usuario y contraseña aleatoria que el usuario puede/debe cambiar en el primer acceso.
3. Crea la estructura de carpetas en la zona de usuario.

## Formato del fichero

El fichero de clientes con extensión csv debe tener la siguiente estructura:

```csv
Abonado;Razón social Cliente;Email empresa
1000;Pepe;miguelns@gmail.com
```

El primero (1000) es el código que recibirá el cliente en nuestra plataforma. Puede ser numérico o alfanumérico. En cualquier caso dicho código será la primera cadena del nombre del fichero de facturas a repartir.

En nuestro ejemplo, las facturas para el usuario 1000 deben empezar por 1000_loquesea.pdf.

Los demás campos se explican por sí mismos.

## Cron

Nuestro script se debe programar con la frecuencia estimada por el administrador de la plataforma atendiendo a sus necesidades. Esto es, si la necesidad de crear clientes nuevos no fuera mucha no tiene sentido establecer la programación con demasiada frecuencia.

En el caso de Hostens, el panel de control nos ofrece un asistente para los trabajos de cron como se muestra a continuación:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-17.33.20.png">
</p>

Fichero de creación de usuarios programado cada 5 minutos.
