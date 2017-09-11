---
layout: post
title: Команды списком к станции Avaya. Часть 1
date: Mon Sep 11 18:00:41 2017 +0200
author: merkulov
tags:
- avaya 
- telephony
comments: true
---
###  Команды списком к станции Avaya. Часть 1

*Он упирается лапками. Сломан язычок. Перепрошить... Понятная рачь специалистов*

Подключитесь через ASA режим Start GEDI к станции.
Горячие кнопки:
__F3__ &mdash; сохранить изменения (Enter)
__Esc__ &mdash; выход
__Enter__ &mdash; продолжить ввод команды
__F5__ &mdash; справка

### 1. Перезагрузка станции 

{% highlight javascript %}
busyout station **** //где **** - тел. номер 
release station ****
save translation
{% endhighlight %}{: .highlight-left }

### 2. Список доступных выходов в город и т.д. 

{% highlight javascript %}
list cor *  //номер cor
// или
list extension-type cor 2 //вывести список по cor 2
// или
list extension-type cor 1 to cor 4 //вывести с 1 по 4
{% endhighlight %}{: .highlight-left }

### 3. Вывести pickup группу и группу переадресации. *Если они есть.* 

{% highlight javascript %}
list groups-ox-extension 1319 (рис.1 1319 - номер телефона, 185 - группа переадресации, 404 - pickup группа)
{% endhighlight %}{: .highlight-left }

![an image alt text](http://lepotuli.ru/merkulov/images/6image1.jpg "рис. 1")

### 4. Операции с тел. станцией

{% highlight javascript %}
add station **** // добавить новую станцию
change station **** // изменить настройки
del station **** //удалить станцию
list station //списком, все записанные номера тел. станций
change pickup-group *** // изменить конкретную pickup группу
change coverage path *** // изменить конкретную группу переадресаций
{% endhighlight %}{: .highlight-left }

### 5. Узнать версию ПО АТС, количество задействованных портов

{% highlight javascript %}
display system-parametrs customer-options (Рис. 2 Здесь V11, 235 - всего портов, 232 - задействованы)
// Количество лицензий высчитывается, если ввести
list conf all // список плат и задействованных портов
{% endhighlight %}{: .highlight-left }

![an image alt text](http://lepotuli.ru/merkulov/images/6image2.jpg "рис. 2")

### 6. Установка времени

{% highlight javascript %}
set time (Рис. 3 Главное указать все параметры)
{% endhighlight %}{: .highlight-left }

![an image alt text](http://lepotuli.ru/merkulov/images/6image3.jpg "рис. 3")

### 7. Перезагрузить порт

{% highlight javascript %}
busyout port 01A0314
release port 01A0314
{% endhighlight %}{: .highlight-left }
