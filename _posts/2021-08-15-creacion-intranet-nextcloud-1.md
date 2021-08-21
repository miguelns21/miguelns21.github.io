---
layout: single
title: Intranet/Nextcloud (Parte 1/4)
excerpt: "Nuestra Intranet parte de una instalación de Nextcloud con varios retoques.
Cualquier proveedor de internet tiene en su panel cpanel/plesk una forma de instalar Nextcloud muy sencilla."
date: 2021-08-15
classes: wide
header:
  teaser: /assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.11.28.png
  teaser_home_page: true
categories:
  - Script
tags:
  - Intranet
  - Nextcloud
---

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.11.28.png">
</p>

Nuestra Intranet parte de una instalación de _Nextcloud_ con varios retoques.

Cualquier proveedor de internet tiene en su panel cpanel/plesk una forma de instalar Nextcloud muy sencilla. En nuestro caso el hosting de pruebas está en 
[hostens](https://www.hostens.com) que para las pruebas iniciales está bien.

## Instalación

La instalación no nos llevará mas que unos cuantos clics. De esta forma ya tenemos nuestro Nextcloud instalado

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.14.29.png">
</p>

## Configuración

Una vez instalado necesitaremos acceder a él y crear el usuario y la contraseña que será el administrador de nuestra intranet. En este momento tenemos Nextcloud instalado pero todavía nos quedan unos ajustes para adaptarlo a nuestras necesidades de Intranet.

### 1. Ponerlo en castellano y habilitar nuestro dominio

Tenemos que modificar/añadir un par de claves que se encuentran en el archivo config/config.ini del directorio de nuestra instalación.

Hostens nos provee en el panel de control de un administrador de ficheros con el que podemos hacer los cambios:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.25.44.png">
</p>

Aquí retocaremos el apartado dominio y el lenguaje:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.27.26-1.png">
</p>

En el array ponemos los dominios desde los que llamaremos a nextcloud.

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.27.36.png">
</p>

Para que todo aparezca en castellano.

### 2. Estructura de carpetas para los usuarios

También hay que preparar el /core/skeleton para que al crear el usuario nos aseguremos que la estructura es correcta.

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.35.54.png">
</p>

Cada usuario tendrá la carpeta Factura en su home.

### 3. Cron

Nextcloud recomienda en su instalación que se programe una tarea de cron en el sistema que lo albergue para su correcto funcionamiento. Aunque existen varias posibilidades, lo mejor, cron.

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.38.35.png">
</p>

Seleccionamos cron en los ajustes del administrador.

Hostens nos provee también de una interfaz muy amigable para establecer la tarea de cron de Nextcloud cada 5 minutos.


<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.44.19.png">
</p>

Acceso a Cron en el panel de Hostens.

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.45.51.png">
</p>


### 4. Configuración de nuestro correo saliente

Necesitaremos una cuenta de email que será el remitente de cuantos correos mandemos a los clientes. Se ha de configurar en los ajustes del administrador de NextCloud (Ajustes básicos):

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.48.20.png">
</p>

### 5. Configurar las aplicaciones instaladas

Por defecto, Nextcloud incluye muchas aplicaciones instaladas que en nuestro caso no son necesarias.

En el panel de control del administrador debemos dejar exclusivamente las aplicaciones que se describen en la siguiente captura:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.52.10.png">
</p>
Aplicaciones instaladas en nuestra intranet.

Todas las demás aplicaciones no aportan para nuestro caso. Las dejaremos deshabilitadas.

### 6. Estructura del usuario administrador

El usuario administrador debe tener una configuración de carpetas como la que se describe en la siguiente imagen:

<p align="center">
<img src="/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-16.55.17.png">
</p>
Estructura de las carpeta en la zona de administración.

1. **Clientes_CSV : Contendrá los ficheros csv con los clientes que nuestros scripts crearán en NextCloud en un formato que se verá mas adelante.
2. **Plantillas_Email: Nuestros scripts pueden mandar correos con un formato preestablecido o bien leer los ficheros html de esta carpeta para mandar emails personalizados.
3. **Reparto_Facturas: El administrador subirá a esta carpeta los ficheros de facturas con formato pdf que nuestros scripts repartirán a los clientes que les correspondan.
4. **scripts: Contiene los scripts python del programa.

