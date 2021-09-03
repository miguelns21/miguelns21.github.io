---
layout: single
title: Notificaciones en tu propio bot de Telegram
excerpt: "Si eres programador o administrador de sistemas sabrás que recibir notificaciones unificadas de los servicios que gestiones es siempre una labor muy repetitiva y rutinaria."
date: 2021-09-02
classes: wide
header:
  teaser: /assets/images/notificaciones_en_bot_telegram/telegram_portada.png
  teaser_home_page: true
categories:
  - Script
tags:
  - Telegram
  - Python
---

# Tu propio bot de notificaciones
Si eres programador o administrador de sistemas sabrás que recibir notificaciones unificadas de los servicios que gestiones es siempre una labor muy repetitiva y rutinaria.

Pues bien en este post voy a describir la forma mas sencilla que he encontrado (de momento) para que puedas mandar notificaciones a tu propio bot de Telegram. Si nunca has creado un bot verás que es un proceso en extremo sencillo.

Vamos manos a la obra.

## 1. Creación de tu propio bot en Telegram

- Accede a tu aplicación de Telegram (web, desktop, móvil, etc.) y busca al usuario @botFather
![](/assets/images/notificaciones_en_bot_telegram/botfather.png)
- Comienza una conversación con él y manda la orden **/newbot**:
![](/assets/images/notificaciones_en_bot_telegram/botfather_conversacion.png)
- Dale un nombre y un usuario a tu bot:
![](/assets/images/notificaciones_en_bot_telegram/botfather_creacion.png)
- @BotFather te contestará con un TOKEN que debes guardar en lugar seguro.
- Busca ahora tu bot y emprende una conversación con él:
![](/assets/images/notificaciones_en_bot_telegram/tu_bot.png)


## 2. Capturar el ID del chat

Necesitas conocer ahora el ID de tu propio chat para ello puedes usar un navegador y poner la siguiente URL:

> https://api.telegram.org/bot[token]/getUpdates

donde token debe ser sustituido por el que ya tenemos.

Tendremos algo del estilo:

<p align="center">
<img src="/assets/images/notificaciones_en_bot_telegram/botfather_id_chat.png">
</p>

## 3. Utilización 

Con estos dos datos en nuestro poder podemos probar nuestro bot de la siguiente forma:

> https://api.telegram.org/bot[token]/sendMessage?chat_id[chatId]&parse_mode=Markdown&text=[msg]

Solo tenemos que sustituir **token** e **chartId** por nuestros valores, siendo **msg** el mensaje que queremos mandar a nuestro bot.

El resultado puede verse a continuación:

<p align="center">
<img src="/assets/images/notificaciones_en_bot_telegram/botfather_pruebas.png">
</p>

Si se quiere usar desde un script en python, aquí tenemos un código de ejemplo:

```python
import requests

emoji_info = 'ℹ️'
emoji_ok = '✅'
emoji_warning = '⚠️'
emoji_error = '❌️'

def sendNotification(notification_body, notification_title, emoji):
    bot_token = '1907783307:AAGqkGE39tJPhD7XXXXXXXXXXXXXX'
    bot_chatID = '262XXXXX'
    msg = emoji + ' *'+notification_title+'*\n\n'+notification_body
    send_text = 'https://api.telegram.org/bot' + bot_token + '/sendMessage?chat_id=' + bot_chatID + '&parse_mode=Markdown&text=' + msg
    response = requests.get(send_text)
    return response.json()

if sendNotification('Cuerpo del mensaje', 'Mi título del mensaje', emoji_info)["ok"]:
    print("Mensaje enviado correctamente")
```

Tendremos que sustituir bot_token y bot_chatID por nuestros valores.

El resultado final:

<p align="center">
<img src="/assets/images/notificaciones_en_bot_telegram/botfather_resultado.png">
</p>

El mundo de los bot es extremadamente amplio y puede resultar muy complejo pero para cubrir exactamente esta necesitad de notificaciones, lo tenemos resuelto.