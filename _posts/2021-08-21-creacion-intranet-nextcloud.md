---
layout: single
title: Creación de Intranet/Nextcloud
excerpt: "Hace un tiempo desarrollé unos scripts en Python para cubrir una necesidad que puede surgir en una empresa pequeña con pocos recursos como es la de dotar a sus propios clientes de un espacio donde éstos puedan consultar sus facturas y descargarlas."
date: 2021-08-21
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

![](/assets/images/creacion-intranet-nextcloud/captura-de-pantalla-2021-08-14-a-las-12.26.26.png)

Hace un tiempo desarrollé unos scripts en Python para cubrir una necesidad que puede surgir en una empresa pequeña con pocos recursos como es la de dotar a sus propios clientes de un espacio donde éstos puedan consultar sus facturas y descargarlas.

## Acciones

Los scripts que funcionarán prácticamente en cualquier hosting o máquina dedicada efectuarán las siguientes acciones:

- Creación de usuarios importando un script csv sencillo. Se mandará al usuario un email con los datos para el primer acceso incluyendo una clave aleatoria.
- Reparto de las facturas en pdf que la empresa subirá a su zona de administración con el formato id_factura.pdf. El sistema notificará al usuario por email de nuevas facturas en su zona de usuario.

## Ventajas

Las ventajas de este sistema son muchas:

1. Aprovechar la infraestructura que crea una software open source como es Nextcloud: seguridad de los datos y simplicidad de manejo.
2. Los documentos sensibles como son las facturas "no viajan" al correo electrónico del usuario, sino que es el cliente final el que accede a sus facturas.
Aislamiento. Aunque Nextcloud es una plataforma en la que se fomenta el intercambio de información entre usuarios también se puede adaptar para que los usuarios estén aislados de los otros.
3. A través de un fichero de configuración de texto, el administrador puede adaptar distintos parámetros de su hosting como son los distintos paths o la configuración de su correo saliente.
4. Los scripts en Python serán publicados en Github para que se puedan adaptar a otras necesidades si así se requiere.

A través de una serie de próximos artículos iré explicando desde la configuración inicial de Nextcloud hasta la configuración de los scripts y su programación en cron.