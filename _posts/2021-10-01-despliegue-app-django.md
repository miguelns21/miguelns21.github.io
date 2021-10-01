---
layout: single
title: Despliegue en producción de una aplicación Django
excerpt: "En este post trato de explicar de la forma mas rápida y concisa cómo poner en producción una aplicación en Django en un VPS basado en Linux."
date: 2021-10-01
classes: wide
header:
  teaser: /assets/images/despliegue-app-django/python-y-django.jpeg
  teaser_home_page: true
categories:
  - Script
  - Administración
tags:
  - Django
  - Python
  - Linux
---

En este post trato de explicar de la forma mas rápida y concisa cómo poner en producción una aplicación en Django en un VPS basado en Linux.

Tómese este post a modo de chuleta con todos los pasos necesarios para el despliegue: desde la instalación de la aplicación partiendo de un repositorio github, la instalación de postgresql, nginx, supervisor, cortafuegos, gunicorn...

Vamos manos a la obra.

## 1. Crear un usuario

Usuario bajo el cual se ejecutará nuestra aplicación:

`adduser jair`

### Dar privilegios sudo al nuevo usuario

Editar con:

`nano /etc/sudoers`

Y añadir la línea:

<p align="center">
<img src="/assets/images/despliegue-app-django/privilegios-usuarios.png">
</p>


## 2. Cambiar el puerto ssh del servidor

Editamos:

`nano /etc/ssh/sshd_config`

<p align="center">
<img src="/assets/images/despliegue-app-django/cambio-puerto.jpeg">
</p>

Reiniciar ssh:

`service ssh restart`

## 3. Configurar el firewall

`ufw status` --> ver el estado

`ufw enable` --> activar firewall

`ufw allow 22` --> permite el paso al puerto 22

`ufw allow ssh` --> permite ssh independientemente del puerto en el que esté configurado.

`ufw status numbered` --> lista las reglas con el orden de aplicación.

`ufw delete 1` --> borra la regla número 1.

`ufw disabe` --> desactiva el firewall.

## 4. Clonar repositorio del proyecto Django

`https://github.com/andrestvp/ver.git`

## 5. Preparar el entorno Python

### Comprobar si se tiene instalado virtualenv:

<p align="center">
<img src="/assets/images/despliegue-app-django/virtualenv.png">
</p>

e instalarlo en su caso:

`sudo apt-get install virtualenv`

### Comprobar si está instalado Python

`python3 --version`

### Crear el entorno virtual en la raíz de la aplicación:

`virtualenv env -ppython3`

El argumento -p servirá para crear el entorno virtual en python3 aunque tuvieramos instalado también python2.

Activamos el entorno virtual con:

`source env/bin/activate`

Instalamos ahora los requerimientos:

`pip install -r requirements.txt`

### Preparar la aplicación

`python manage.py makemigrations`
`python manage.py migrate`
`python manage.py createsuperuser`

## 6. Instalar Postgresql

`sudo apt install postgresql postgresql-contrib`

Configurar postgresql:

`sudo -u postgres psql`

Cambiar la contraseña del usuario postgres:

<p align="center">
<img src="/assets/images/despliegue-app-django/configuracion-postgresql.png">
</p>

Cambiar la codificación de caracteres y la zona horaria:

`ALTER ROLE postgres SET client_encoding TO 'utf8';`
`ALTER ROLE postgres SET timezone TO 'UTC';`

Crear una base de datos:

`create database apolo;`

<p align="center">
<img src="/assets/images/despliegue-app-django/database-postgresql.png">
</p>

Verificamos en nuestro entorno virtual de python si tenemos instalada la librería de postgresql:

`pip list`

En caso de que no:

`pip install psycopg2-binary`

Cambiamos la configuración del proyecto django a postgresql.

`nano config/db.py`

verificando usuario y contraseña.
De nuevo haremos migraciones y migrate.

## 7. Crear un fichero bash para ejecutar nuestra app python

Crearemos una carpeta 'deploy' en la estructura de nuestra app que contendrá el bash.

Para saber mas sobre bash hay un buen tutorial en [Tutorial Bash](https://bioinf.comav.upv.es/courses/unix/scripts_bash.html)

El script es 'server.sh':

```bash
#!/bin/bash
DJANGODIR=$(dirname $(cd `dirname $0` && pwd))
DJANGO_SETTINGS_MODULE=config.settings
cd $DJANGODIR
source env/bin/activate
export DJANGO_SETTINGS_MODULE=$DJANGO_SETTINGS_MODULE
exec python manage.py runserver 0:9000
```

### Configurar Supervisor (gestor de procesos)

Se encargará de mantener nuestra aplicación siempre corriendo en nuestro servidor.

1.  Instalar supervidor
    `apt-get install supervisor`
    
2.  Crear el siguiente archivo conf en la ruta */etc/supervisor/conf.d/apolo.conf*
    

```
[program:apolo]
command=/home/jair/apolo/deploy/server.sh
autostart=true
autorestart=true
stderr_logfile=/home/jair/apolo/logs/err.log
stdout_logfile=/home/jair/apolo/logs/out.log
user=jair
environment=LANG=en_US.UTF-8,LC_ALL=en_US.UTF-8
```

Comprobamos ahora si un fichero de supervisor es correcto:

`sudo supervisorctl reread`

Ahora actualizamos las tareas de supervisor:

`sudo supervisorctl update`

Podemos ver todo lo que se está ejecutando con supervisor:

`sudo supervisorctl status`

Para parar el servicio:

`sudo supervisorctl stop apolo`

Para iniciarlo:

`sudo supervisorctl start apolo`

Para reiniciar una tarea:

`sudo supervisorctl restart apolo`

o todas:

`sudo supervisorctl restart all`

Si hacemos un cambio al script debemos actualizarlo en supervisor:

`sudo supervisorctl update`

## 8. Configurar proxy gunicorn

<p align="center">
<img src="/assets/images/despliegue-app-django/estructura-django.png">
</p>

Instalamos en nuestro entorno virtual:

`pip install gunicorn`

Creamos en la carpeta config un fichero settings llamado *production.py* que añadirá un par de variables a nuestro setting original:

```
from .settings import *
ALLOWED_HOSTS = ["192.168.0.9", "midominio.com", "www.midominio.com"]
DEBUG = False
```

Editamos el archivo *wsgi.py* para que apunte al nuevo *production.py*

<p align="center">
<img src="/assets/images/despliegue-app-django/wsgi.png">
</p>

Para comprobar que está funcionando gunicorn:

`gunicorn config.wsgi`

desde la ruta del programa:

<p align="center">
<img src="/assets/images/despliegue-app-django/estado-gunicorn.png">
</p>

**Importante** gunicorn servirá sobre el puerto 8000 por lo que tendremos que para el supervisor con:

`sudo supervisorctl stop apolo`

Necesitaremos un fichero bash para que que supervisor levante gunicorn:

```bash
#!/bin/bash

NAME="apolo"
DJANGODIR=$(dirname $(cd `dirname $0` && pwd))
SOCKFILE=/tmp/gunicorn-apolo.sock
LOGDIR=${DJANGODIR}/logs/gunicorn.log
USER=jair
GROUP=jair
NUM_WORKERS=5
DJANGO_WSGI_MODULE=config.wsgi

rm -frv $SOCKFILE

echo $DJANGODIR

cd $DJANGODIR

exec ${DJANGODIR}/env/bin/gunicorn ${DJANGO_WSGI_MODULE}:application \
  --name $NAME \
  --workers $NUM_WORKERS \
  --user=$USER --group=$GROUP \
  --bind=unix:$SOCKFILE \
  --log-level=debug \
  --log-file=$LOGDIR
```

Lo llamaremos *gunicorn.sh* y estará en la carpeta deploy con permisos de ejecución.

## 9. Configurar web server

`sudo apt install nginx`

Si usamos firewall habilitamos nginx:

`sudo ufw allow 'Nginx HTTP'`

`sudo ufw allow 'Nginx HTTPS'`

Verificamos que está corriendo:

`systemctl status nginx`

En el entorno virtual de python ejecutaremos:

`python manage.py collectstatic --link --noinput`

para organizar todas los recursos estáticos en una sola carpeta pero con enlaces simbólicos (sin ocupar espacio en disco).

Creamos el fichero de nginx en la ruta */etc/nginx/sites-available/apolo.conf' con el contenido:

```bash
upstream apoloconn {
    server unix:/tmp/gunicorn-apolo.sock fail_timeout=0;
}

server {
    listen 80;
    server_name www.algorisoftprojects.com algorisoftprojects.com 192.168.0.9;

    access_log /home/jair/apolo/logs/nginx-access.log;

    error_log /home/jair/apolo/logs/nginx-error.log;

    location /media/  {
        alias /home/jair/apolo/media/;
    }

    location /static/ {
        alias /home/jair/apolo/staticfiles/;
    }

    location /static/admin/ {
        alias /home/jair/apolo/staticfiles/admin/;
    }

    location / {
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_set_header Host $http_host;
         proxy_redirect off;
         proxy_pass http://apoloconn;
    }

    error_page 500 502 503 504 /templates/500.html;
}
```

Habilitamos el sitio haciendo un enlace simbólico:

`sudo ln -s /etc/nginx/sites-available/apolo.conf /etc/nginx/sites_enabled/`

Comprobamos la configuración creada con:

`sudo nginx -t`

Cambiamos ahora nuesto *apolo.conf* de supervisor para que ahora apunto a gunicorn.sh en lugar de a server.sh como lo venía haciendo hasta este momento:

`sudo nano /etc/supervisor/conf.d/apolo.conf`

<p align="center">
<img src="/assets/images/despliegue-app-django/cambio-a-gunicorn.png">
</p>

Actualizamos supervisor y reiniciamos nginx:

`sudo supervisorctl update`
`sudo systemctl restart nginx`

Ahora es recomendable eliminar el puerto 8000 de las reglas del cortafueros:

`sudo ufw status numbered`

<p align="center">
<img src="/assets/images/despliegue-app-django/actualizacion-ufw.png">
</p>

`sudo ufw delete 2`

## 10. Instalar los certificados SSL

Primero instalaremos el software certbot en el servidor:

`sudo apt install certbot python3-certbot-nginx`

Una vez instalado:

`sudo certbot --nginx -d midominio.com -d www.midominio.com`

A la pregunta sobre redireccionar http a https contestamos '2' (si).

Actualizamos nuestro cortafuegos:

`sudo ufw allow 'Nginx Full`
`sudo ufw delete allow 'Nginx HTTP`

## 11. Backup de la Base de datos

Crearemos un script bash para hacer el backup de postgresql:

```bash
#!/bin/bash

export FECHA=`date +%d_%m_%Y_%H_%M_%S`
export NAME=apolo_${FECHA}.dump
export DIR=/home/jair/backup/
USER_DB=postgres
NAME_DB=apolo
cd $DIR
> ${NAME}
export PGPASSWORD=123
chmod 777 ${NAME}
echo "procesando la copia de la base de datos"
pg_dump -U $USER_DB -h localhost --port 5432 -f ${NAME} $NAME_DB
echo "backup terminado"
```

Añadimos una tarea a cron:

`crontab -e`

Programamos la tarea en un tiempo determinado:

`* * * * * bash /home/jair/apollo/deploy/backup.sh`

Verificamos la tarea creada:

`crontab -l`