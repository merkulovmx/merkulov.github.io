---
layout: post
title: Загрузка новой конфигурации ключей на межсетевой экран DionisNX
date: Mon Oct 9 17:07:00 2017 +0200
author: merkulov
tags:
- dionis
- firewall
comments: true
---
### Загрузка новой конфигурации ключей на межсетевой экран DionisNX, удаленное подключение 

*Наблюдая за каплями дождя, бьющими по лужам, кажется, что дождь играет в змейку или/и тетрис*

0. Для начала необходимо расшарить папку с конфигами. Используем __tftpd__ клиент.
- Скачиваем [tftpd](http://www.jounin.net/tftpd32.html)
- Указываем путь к папке (рис. 1 Пункт 1)
- IP (рис. 1 Пункт 2)

![an image alt text](http://lepotuli.ru/merkulov/images/10image1.jpg "рис. 1 Параметры сессии в tftpd")


1. Подключаемся удаленно к DionisNX, через Putty (см. настройку в посте ["Загрузить ключ на DionisNX c flash-карты"](http://lepotuli.ru/merkulov/2017/09/27/Dionis_new_key.html))

{% highlight javascript %}
login as: login
Welcome to DionisNX
 password: *********
DionisNX# copy startup-config tftp://10.78.26.111/new_conf_name.cfg config-new  // копируем конфиг
Info: copying 'tftp://10.78.26.111/new_conf_name.cfg' to 'file:config-new'...
DionisNX# copy config-new startup-config  // загружаем в startup 
Info: copying 'file:config-new' to 'startup-config'...
DionisNX# reboot  // перезагрузка
Info: The running-config is not equal to startup-config.
Warning: Host 'DionisNX' will be rebooted!
[y/n]?y
// Подключаемся еще раз
login as: login
Welcome to DionisNX
 password: *********
DionisNX# show crypto disec conn *  // посмотреть список туннелей

// [#]NAME – имя туннеля, если перед именем стоит #, то туннель выключен.
// ID – идентификатор туннеля.
// SRC, DST – адреса локального и удаленного концов туннеля.
// SN, LOC и REM – серия и криптономера ключей шифрования.
// A – алгоритм: E – шифрование, C – сжатие, B – шифрование и сжатие, N – отсутствует.
// B – блокировка туннеля: Y – есть, N – нет.

[#!]NAME        ID    SRC             DST             SN         LOC   REM   A B
t1              1   172.16.31.2   172.16.32.2         222         1     2    B N
DionisNX# exit
{% endhighlight %}{: .highlight-left }
