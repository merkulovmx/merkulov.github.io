---
layout: post
title: Меняем тональность Avaya
date: Mon Oct 2 17:17:00 2017 +0200
author: merkulov
tags:
- avaya 
- telephony
comments: true
---
### Меняем тональность (длину гудка) для входящих звонков Avaya

*&mdash Зачем вам это?*
*&mdash Для возможности быстрого определения источника звонка*

Подключаемся к станции через ASA(Avaya Site Administration) __используем режим Start GEDI__.
- В командной строке вводим команду __change system-parameters features__, тем самым вызываем меню параметров системы для отдельных функций
{% highlight javascript %}
change system-parameters features
{% endhighlight %}{: .highlight-left }
- Переходим на вкладку 5
- Distinctive Audible Alerting: Internal, External, Priority (рис1. Настройки звукового сигнала для приоритетов)

![an image alt text](http://lepotuli.ru/merkulov/images/10image1.jpg "рис. 1 Настройки звукового сигнала для приоритетов")

- В командной строке вводим команду __change feature-access-codes__, настройки перенаправления
{% highlight javascript %}
change feature-access-codes
{% endhighlight %}{: .highlight-left }
- Переходим на вкладку 3
- Приоритетный звонок выставляется в графе __Priority Calling Access Code__ (рис. 2 Настройка приоритетного звонка)

![an image alt text](http://lepotuli.ru/merkulov/images/10image2.jpg "рис. 2 Настройка приоритетного звонка")
