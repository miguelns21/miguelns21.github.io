---
layout: single
title: Linux Smart Enumeration - Herramienta para pentesting
excerpt: "Programar un script que hace algo pero que es difícil comprobar el resultado es una práctica poco segura."
date: 2021-08-25
classes: wide
header:
  teaser: /assets/images/linux-smart-enumeration/linux-terminal.jpg
  teaser_home_page: true
categories:
  - Script
tags:
  - Linux
  - Pentesting
---

![](/assets/images/linux-smart-enumeration/lse_level0.png)

Empezamos con unos oneliners rápidos:

`wget "https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh" -O lse.sh;chmod 700 lse.sh`

`curl "https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh" -Lo lse.sh;chmod 700 lse.sh`


# linux-smart-enumeration
Linux Smart Enumeration - Herramienta para pentesting

Este proyecto está inspirado por https://github.com/rebootuser/LinEnum y usa la mayoría de sus tests.

A diferencia de LinEnum, `lse` trata de mostrar la información ordenada desde el punto de vista de su importancia en cuanto a privilegios se refiere.

## ¿Qué es?

Este script de shell muestra información relevante sobre la seguridad de un sistema linux local ayudándo a escalar privilegios.

También puede **monitorizar procesos para descubrir ejecuciones de procesos recurrentes**. Monitoriza al mismo tiempo que ejecuta todos los demás test. Por defecto monitoriza durante 1 minuto pero puede cambiarse este tiempo a través del parámetro `-p`.

Tiene 3 niveles de información a mostrar para controlar cuanta información se quiere ver.

## ¿Cómo se usa?

La idea es obtener información gradualmente.

Primero se debería ejecutar como `./lse.sh`. Si se ven algunos `yes!`, probablemente sea información interesante para trabajar sobre ella.

Si no, se puede intentar ejecutar con el primer nivel de salida del comando `level 1` con `./lse.sh -l1` y se mostrará mas información interesante.

Si esto tampoco ayuda, `level 2` mostrará toda la información recolectada sobre el servicio usando `./lse.sh -l2`. En este caso será importante usarlo de la siguiente forma `./lse.sh -l2 | less -r`.

Se pueden seleccionar qué tipo de tests ejecutar a través del parámetros `-s`. Por ejemplo `./lse.sh -l2 -s usr010,net,pro` ejecutará el teste `usr010` y todos los tests de la sección `net` and `pro`. 


```
Use: ./lse.sh [options]

 OPTIONS
  -c           Disable color
  -i           Non interactive mode
  -h           This help
  -l LEVEL     Output verbosity level
                 0: Show highly important results. (default)
                 1: Show interesting results.
                 2: Show all gathered information.
  -s SELECTION Comma separated list of sections or tests to run. Available
               sections:
                 usr: User related tests.
                 sud: Sudo related tests.
                 fst: File system related tests.
                 sys: System related tests.
                 sec: Security measures related tests.
                 ret: Recurren tasks (cron, timers) related tests.
                 net: Network related tests.
                 srv: Services related tests.
                 pro: Processes related tests.
                 sof: Software related tests.
                 ctn: Container (docker, lxc) related tests.
               Specific tests can be used with their IDs (i.e.: usr020,sud)
  -e PATHS     Comma separated list of paths to exclude. This allows you
               to do faster scans at the cost of completeness
  -p SECONDS   Time that the process monitor will spend watching for
               processes. A value of 0 will disable any watch (default: 60)
  -S           Serve the lse.sh script in this host so it can be retrieved
               from a remote host.
```

### Demo

![LSE Demostración](https://github.com/miguelns21/linux-smart-enumeration/raw/master/screenshots/lse.gif)

### Level 0 (default) ejemplo de salida

![LSE level0](https://raw.githubusercontent.com/miguelns21/linux-smart-enumeration/master/screenshots/lse_level0.png)


## Ejemplos

Ejecución directa en una línea

`bash <(wget -q -O - https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh) -l2 -i`

`bash <(curl -s https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh) -l1 -i`
 
La fuente original de esta información en inglés se puede encontrar en [hakin9.org](https://hakin9.org/linux-smart-enumeration/).