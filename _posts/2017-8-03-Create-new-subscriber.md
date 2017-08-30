---
layout: post
title: Снять номер с порта и создать новый телефонный номер, задать имя пользователя на Nortel Meridian
date: Wed Aug 30 17:33:41 2017 +0200
author: merkulov
tags:
- nortel
- telephony
comments: true
---
### Снять номер с порта и создать новый телефонный номер, задать имя пользователя на Nortel Meridian
*"Наконец-то я поднял блог и смогу написать то, что не вмещается в тетради."*
#### Подключаемся. Логинимся 

```javascript
logi Имя_пользователя
Ввести пароль
```

1. Прежде чем выполнять удаление стоит снять резервный лог интересующей нас информации со станции.
Например пусть это будет номер телефона 2057

{% highlight javascript %}
>LD 20

REQ: prt 
TYPE: dn
TYPE DNB
CUST 0
DN   2057
DATE 
PAGE 
DES 

DN   2057
TYPE SL1 
TN   014 0 00 03 KEY 00   MARP  DES 3902      11 APR 2009 
     (3902)
{% endhighlight %}{: .highlight-left }

Из выведенной информации видно, что номер 2057 - этот аппарат цифровой, ведь имеет цифровой порт. (3902 - цифра, 500 - аналог)
Подключен на порт 14 3
Выведем конфигурации аппарата.
{% highlight javascript %}
>LD 20

REQ: prt
TYPE: tn
TYPE TNB
TN   14 3
DATE 
PAGE 
DES  

DES  3902  
TN   014 0 00 03 
TYPE 3902
CDEN 8D
CUST 0 
ERL  0 
FDN  
TGAR 0 
LDN  NO
NCOS 1 
SGRP 0 
RNPG 718 
SCI  0 
SSU  
XLST 
SCPW 
SFLT NO
CAC_CIS 3 
CAC_MFC 0
CLS  CTD FBD WTA LPR PUA MTD FND HTD TDD HFD GRLD 
     MWD LMPN RMMD SMWD AAD IMD XHD IRD NID OLD VCE DRG1
     POD DSX VMD SLKD CCSD SWD LND CNDD
     CFTD SFD MRD DDV CNID CDCA MSID DAPA BFED RCBD 
     ICDD CDMD MCTD CLBD AUTU
     GPUD DPUD DNDD CFXD ARHD CLTD ASCD 
     CPFA CPTA ABDD CFHD FICD NAID DNAA RDLA BUZZ AGRD MOAD 
     UDI RCC HBTD AHD IPND  DDGA NAMA MIND PRSD NRWD NRCD NROD 
     DRDD EXR0 
     USRD ULAD RTDD RBDD RBHD PGND OCBD FLXD FTTC DNDY DNO3 MCBN 
     FDSD NOVD CDMR MCDD T87D PKCH 
CPND_LANG ENG
HUNT 
PLEV 02 
DANI NO
AST  
IAPG 0 
AACS NO
ITNA NO  
DGRP 
MLWU_LANG 0 
MLNG RUS
DNDR 0 
KEY  00 SCR 2057 0     MARP
        ANIE 0 
     01 RNP 
     02 ADL 16  
     03 CFW 16  
     04 TRN 
     05     
DATE 22 APR 2016 
{% endhighlight %}{: .highlight-left }
Сохраняем на всякий случай. [Инструкция по конфигурационным командам](https://yadi.sk/i/cEUjXcoo3MTNAh)

2. Удаляем номер 2057
{% highlight javascript %}
>LD 20

REQ: out 
TYPE: 3902
TN   14 3
{% endhighlight %}{: .highlight-left }

2. Создаем номер 2564 на освободившийся порт 14 3
{% highlight javascript %}
ld 20

PT0000 
REQ: new
TYPE: 3902
TN   14 3
DES  3902
CUST 0
ERL 0
FDN  
TGAR 0
LDN  no
NCOS 1
RNPG 23
SSU  
XLST 1
SCPW 
SGRP 0
SFLT no
CAC_CIS 3
CAC_MFC 0
CLS  
HUNT 
SCI  0
PLEV 02
DANI no
AST  
IAPG 0
MLWU_LANG 0
MLNG rus
DNDR 0
KEY 00 scr 2564 0
  MARP
  CPND 
  VMB 
  ANIE 0
KEY 

MGMT001 TNB NEW TYPE:3902 TN:14 0 0 3

//Нажимаем Enter до строки информирующей о записи данных

MEM AVAIL: (U/P): 2718916    USED U P: 220133 59223    TOT: 2998272 
{% endhighlight %}{: .highlight-left }

3. После стоит отредактировать __CLS__ параметры
Например для модели 3904 разрешая многое. Для 3902 почти тоже самое в зависимости от задач.

{% highlight javascript %}
>ld 20

REQ: chg
TYPE: 3904
TYPE: 500

TN   22 11
ECHG yes
ITEM cls TLD PUA FNA HFA OLA LNA CNDA 
CFTA ICDA CDMA MCTA 
GPUA DPUA DNDA CFHA NAIA
ITEM 

//Нажимаем __Enter__ до строки информирующей о записи данных

MGMT001 TNB CHG TYPE:3904  TN:14 0 0 3
{% endhighlight %}{: .highlight-left }

4. Задать имя пользователя, через LD 95

{% highlight javascript %}
ld 95

REQ  chg
TYPE name
CUST 0
  CPND_LANG 
DN   2564
  NAME Merkulov M. A.
  DISPLAY_FMT 
{% endhighlight %}{: .highlight-left }

Всё. Можно проверить, что ввели через команду __LD 20__.
Если в процессе ввода допускаете ошибку, то можно отменить действие через ввод __**__ или полностью выйти из команды __****__
#### Сохранить изменения с помощью __LD 43__ (после точки ввести __edd__)
#### Чтобы завершить работу со станцией нужно набрать команду __logo__ и затем только нажать __Disconnect__. (рис. 6)
