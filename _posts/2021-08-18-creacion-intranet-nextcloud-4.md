---
layout: single
title: Intranet/Nextcloud (Parte 4/4)
excerpt: "Programar un script que hace algo pero que es difícil comprobar el resultado es una práctica poco segura."
date: 2021-08-18
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


# Consideraciones finales

Programar un script que hace algo pero que es difícil comprobar el resultado es una práctica poco segura.

Nuestros scripts incorporan la salida de todos los procesos que llevan a cabo a un fichero que se situará en la raíz del la zona del usuario administrador.

Este fichero de log registra todos los movimientos que el script ha llevado a cabo o no por distintos motivos. Los principales controles que registra son:

1. Incorrecto formato del fichero de clientes CSV
2. Creación de usuario correcta o no.
3. Factura repartida a qué usuario.
4. etc.

Espero que estos scripts puedan servir de base o ayudar al desarrollo de una versión adaptada a las necesidades de cualquier pequeña empresa.

Los scripts están disponibles en mi [Github](https://github.com/miguelns21/NextIntranet).