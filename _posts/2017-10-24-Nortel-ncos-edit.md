---
layout: post
title: Просмотр ограничений и установка сетевого типа услуг ncos на Nortel
date: Tue Oct 24 12:00:41 2017 +0200
author: merkulov
tags:
- nortel
- telephony
comments: true
---
### Просмотр ограничений и установка сетевого класса обслуживания (ncos) на Nortel Meridian

*Через старое к новому*

### 1. Выведем с помощью команды __ld 81__ список ncos
Обратимся к справочнику:
*Оверлейная программа 81 используется, чтобы распечатывать список или счет телефонов с выбранными возможностями. Это также позволяет распечатывать информацию о дате последней замены услуги.  *
{% highlight javascript %}
>ld 81

REQ   lst
CUST  0
DATE  
PAGE  
DES   
FEAT  ncos
NCOS  
FEAT  

NCOS     00         000     TN  004 0 00 01  3900      3902      10 JUN 2008 
NCOS     00         000     TN  004 0 00 02  3900      M3904     31 MAR 2017 
NCOS     00         000     TN  018 0 00 00  2000      CP        17 JUL 2006 
NCOS     00 6532    001     TN  017 0 00 12  2500      500       14 SEP 2009 
NCOS     00 6514    001     TN  017 0 00 14  2500      500       27 JUN 2008 
{% endhighlight %}{: .highlight-left }

### 2. Узнав какие номера есть и что за ограничения они используют. Выведем существующие таблицы ограничений __ld 49__

{% highlight javascript %}
>ld 49

REQ  prt
TYPE fcr
CUST 0
CRNO 

CRNO   0
INIT ALOW
DENY 98 // запрещено
ALOW 988001 //разрешено
     988002
     988003
     988004
     988005
     988006
     988007

CRNO   1
INIT ALOW

CRNO   2

CRNO   3

CRNO   4
{% endhighlight %}{: .highlight-left }

### 3. Например, хотим полностью изменить ограничения __ncos 0__ Все действия происходят в программе __ld 49__

{% highlight javascript %}
>ld 49

// можно использовать изменение существующего списка ограничений
LD 49

REQ  chg
TYPE fcr
CUST 0
CRNO 0
DENY 9
UPDT 
FRCE 
DENY 
ALOW 
BYPS 

// Лучше произвести удаление и всё перезаписать, т.к. иногда внести изменения в существующие ограничения система не дает
REQ  out // производим удаление
TYPE fcr
CUST 0
CRNO 0 // указываем ncos

MEM AVAIL: (U/P): 2604165    USED U P: 378565 64693    TOT: 3047423 
DISK RECS AVAIL: 1152 

REQ  prt
TYPE fcr
CUST 0
CRNO 

CRNO   0 // проверяем, отсутствуют ограничения, разрешения для ncos 0

CRNO   1
INIT ALOW

CRNO   2

CRNO   3

CRNO   4

// создаем ограничения для __ncos 0__ Например, запрет девятки
REQ  new
TYPE fcr
CUST 0
CRNO 0
INIT alow
DENY 9
UPDT 
DENY 
ALOW 
BYPS 

MEM AVAIL: (U/P): 2604150    USED U P: 378565 64708    TOT: 3047423 
DISK RECS AVAIL: 1152 

// Проверяем наличие внесенных изменений
REQ  prt
TYPE fcr
CUST 0
CRNO 

CRNO   0
INIT ALOW
DENY 9

CRNO   1
INIT ALOW

CRNO   2

CRNO   3

CRNO   4
{% endhighlight %}{: .highlight-left }

#### Список использованных источников
1. 553-3001-306 "Администрирование X11"
2. 553-3001-311 Стандарт 7.00 Апрель 2000
