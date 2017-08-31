---
layout: post
title: Группы перехвата, измененить время, разблокировать порт Nortel Meridian
date: Thur Aug 31 15:00:00 2017 +0200
author: merkulov
tags:
- nortel
- telephony
comments: true
---
### Группы перехвата, измененить время, разблокировать порт Nortel Meridian

*Когда работа оценивается не качеством, а количеством...*

#### 1. Группы перехвата
**Просмотр групп перехвата**
{% highlight javascript %}
>LD 81

REQ: LST 
CUST: 0
DATE:
PAGE:
DES:
FEAT: RNP   // или PUA
RNPG: all //1 100 или +
{% endhighlight %}{: .highlight-left }
**Изменение группы перехвата для конкретного номера**
{% highlight javascript %}
>LD 20

REQ: chg 
TYPE: 500 //3902, 3904
TN: 5 5
ECHG: yes
ITEM: RNPG *
{% endhighlight %}{: .highlight-left }
#### 2. Изменить время на станции
{% highlight javascript %}
>LD 02

.STAD "24 11 1976 15 41 49"

// 24 - день, 11 - месяц, 1976 - год, 15 41 49 - ч м сек
//ttad - вывести текущее время станции 
{% endhighlight %}{: .highlight-left }
#### 3. Разблокировать телефон
**Когда продолжительное время не используется порт, то происходит его блокировка.**
{% highlight javascript %}
>LD 20

REQ: disu 7 8  // заблокировать (где 7 8 - это TN)

REQ: stat 7 8 // статус 

DSBL 
REQ: enlu 7 8 // разблокировать
{% endhighlight %}{: .highlight-left }
#### 4. Незадействованные номера (DN)
{% highlight javascript %}
>LD 20

REQ: prt
TYPE: LUDN
{% endhighlight %}{: .highlight-left }
#### 5. Незадействованный порт (TN)
{% highlight javascript %}
>LD 20

REQ: LUVU
TYPE: 2000 // - цифра, 500 - аналог
{% endhighlight %}{: .highlight-left }
#### 6. Незадействованный порт вывести таблицей
{% highlight javascript %}
>LD 83

REQ: lst
CUST: 0
{% endhighlight %}{: .highlight-left }
На выходе будет таблица вида:
| Аппарат | Дата создания | TN | Модель | Номер телефона |
#### 7. Вывести список всего ncos (Параметр разрешения/ограничения выхода пользователя дальше в сеть)
{% highlight javascript %}
>LD 81

REQ: lst
CUST: 0
FEAT: ncos
NCOS: // - пустой, то весь список
// 2 - выход в город
// 4 - межгород
// 1 - местные и т.д. зависят от настройки
{% endhighlight %}{: .highlight-left }
