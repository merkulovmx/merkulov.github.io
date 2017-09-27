---
layout: post
title: Загрузить ключ на DionisNX c flash-карты
date: Tue Sep 27 17:00:41 2017 +0200
author: merkulov
tags:
- dionis
- firewall
comments: true
---
### Загрузка ключей на межсетевой экран DionisNX с flash-карты вставленной в него

*!Смену ключей стоит производить с осторожностью*

1. Подключаемся удаленно к DionisNX, через Putty
Cкачиваем и запускаем [Putty](http://www.putty.org/)
- настраиваем сессию. Указывваем IP _xx.0.*.7_ Тип подключения SSH. Порт 22 (рис. 1 Параметры сессии в Putty) 

![an image alt text](http://lepotuli.ru/merkulov/images/9image1.jpg "рис. 1 Параметры сессии в Putty")

- обязательно разрешаем ведение лога сессии (рис. 2 Настройки для лога)

![an image alt text](http://lepotuli.ru/merkulov/images/9image2.jpg "рис. 2 Настройки для лога")

2. Подключаемся к удаленному Dionis.
*!Если Dionis в режиме master, то ключ необходимо установить и на slave*
{% highlight javascript %}
DionisNX[master]#cluster connect
{% endhighlight %}{: .highlight-left }

- Вводим логин, пароль, смотрим наличие flash-карты, сверяем версию ключей, загружаем, сохраняем

{% highlight javascript %}
login as: login
Welcome to DionisNX
 password: *********
DionisNX# show crypto disec key *  // смотрим установленные ключи на Dionis
Installed keys:
Serial: XXX0 , locals: ###0
DionisNX# ls /  // проверяем наличие flash-карты
flash0:
file:
share:
log:
DionisNX# ls flash0: // смотрим, что есть на flash-карте
drwxrwxr-x    3 root     adm         4.0K Sep 13 10:31 ###0/
DionisNX# crypto disec import key flash //добавляем новый ключ
Info: key (serial:XXX1; cn:###1) successfully added
DionisNX# crypto disec key * // смотрим установленные ключи на Dionis
Installed keys:
Serial: XXX0 , locals: ###0
Serial: XXX1 , locals: ###1
@DionisNX# write // запись
Info: copying 'running-config' to 'startup-config'...
{% endhighlight %}{: .highlight-left }
