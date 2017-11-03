---
layout: post
title: Поменять аналог на цифру через свободный номер на Nortel
date: Fri Nov 3 15:02:41 2017 +0200
author: merkulov
tags:
- nortel
- telephony
comments: true
---
### Поменять аналог на цифру через свободный номер телефона на Nortel Meridian

Задача: поменять местами номер 2183 (аналоговый) и 2186 (цифровой). 

#### Вообще существует три варианта 
1. Можно перекроссировать в серверной.
2. Удалить номера и заново создать на необходимых TN. Пост "[Снять номер с порта и создать новый телефонный номер, задать имя пользователя на Nortel Meridian](https://merkulovmx.github.io/2017/08/30/Create-new-subscriber.html)"
3. Аналог на цифру через незадействованный номер телефона.

Если с 1 и 2 вариантами вполне всё понятно, то вариант 3 в некоторых случаях более быстрый.
Реализуем нашу задачу через этот вариант. 

### 1. Удаленно подключаемся к станции Nortel Meridian.
Пост "[Как подключиться к станции Nortel Meridian](https://merkulovmx.github.io/2017/01/23/Connect-to-Nortel-Meridian.html)"
#### Для начала посмотрим настройки номеров, на каких портах расположены номера, типы аппаратов. Программа __ld 20__ 

{% highlight javascript %}
>logi Логин
PASS? Пароль

>ld 20

REQ: prt
TYPE: dn
TYPE DNB
CUST 0
DN 2186
DATE 
PAGE 
DES 

DN   2186
TYPE SL1 
TN   006 0 00 12 KEY 00   MARP  DES 3902       6 OCT 2009 
     (3902)


REQ: prt
TYPE: dn
TYPE DNB
CUST 0
DN   2183
DATE 
PAGE 
DES 

DN   2183
TYPE L500
TN   024 0 00 01  MARP          DES 500        7 OCT 2014 
{% endhighlight %}{: .highlight-left }

### 2. В результате нам известно, что 
- 2183 имеет настройки аналогового типа аппарата 500 и  находится на TN 24 1 
- 2186 имеет настройки цифрового типа аппарата 3902 и  находится на TN 6 12 
#### Выясним в какие группы переадресации они включены. И удалим эти номера из найденных групп. Программа __ld 20__ и __ld 18__
*!Перед операциями изменения обязательно нужно вывести номера из групп переадресации. Иначе при попытке изменения выйдет ошибка __"ITEM dn SCH0101"__*

{% highlight javascript %}
>ld 20
REQ: prt
TYPE: ght
LSNO 

// выведется большой список групп. Я ищу через Ctrl+F в логе со станции. Но наши номера в итоге входят в одну группу 0011
GHLN 0011 
GHT  
     
PLDN 0705
DNSZ 4 
STOR 0 2183 // наш аналог 
STOR 1 2179
STOR 2 2186 // наша цифра
STOR 3 2198

**** // выходим из программы __ld 20__ 
// Заходим в __ld 18__ для редактирования группы переадресации 0011
>ld 18

REQ  chg
TYPE ght
LSNO 0011
SIZE                            // пропускаем
WRT  (ADDS: MEM: 0 DISK: 0.0)
STOR 0                          // вводим 0 и два раза пробел, Enter             
WRT  (ADDS: MEM: 0 DISK: 0.0)
STOR 2                          // вводим 2 и два раза пробел, Enter    
WRT  

**** // выходим из программы __ld 18__ 
// проверим изменения в группе __ld 20__ 
>ld 20
REQ: prt
TYPE: ght
LSNO 0011
SIZE 4 
RNGE 
     
GHLN 0011 
GHT  
     
PLDN 0705
DNSZ 4 
STOR 0 
STOR 1 2179
STOR 2 
STOR 3 2198

{% endhighlight %}{: .highlight-left }

### 3. Найдем свободный номер ,через который будем проводит операцию. Программа __ld 20__
{% highlight javascript %}
>ld 20
REQ: prt
TYPE: ludn
CUST 0
DN   

CUSTOMER 00  - UNUSED DNS:
0002    0003    0004    0005    0010    0011    0012    0013    0014    0015
0016    0019    0670    0671    0672    0673    0674    0675    0676    0679
0687    0691    0694    0711    0749    075     076     077     078     079
2506    2507    2508    2509    2522    2523    2524    2528    2529    2530
2531    2533    2534    2535    2536    2537    2538    2539    2540    2541
2542    2543    2544    2545    2546    2547    2549    *00     *01     *02
*03     *04     *05     *07     *08     *09     *10     *11     *12     *130
*131    *132    *134    *135    *136    *137    *138    *139    *14     *16
*17     *18     *19     *2      *3      *4      *5      *8      *9      #00
#01     #02     #04     #05     #06     #07     #08     #09     #5      #8
#9      

// возьмем 2506. Можно сразу же проверить отсутствие номера
REQ: prt
TYPE: dn
TYPE DNB
CUST 0
DN   2506
DATE 
PAGE 
DES 

SCH0881                         // такого номера нет

NO ACT SINCE NO DATE     
{% endhighlight %}{: .highlight-left }

### 4. Нам предстоит сделать следующие изменения:
- 2183 (аналог TN 24 1) на номер 2506 (свободный)
- 2186 (цифровой TN 6 12) на номер 2183 
- 2506 (аналог TN 24 1) на номер 2186
Программа __ld 20__

{% highlight javascript %}
> ld 20

// выполняем первый пункт "2183 (аналог TN 24 1) на номер 2506 (свободный)"
REQ: chg
TYPE: 500                 // аналог типа 500 
TN   24 1                 // порт 24 1
ECHG yes
ITEM dn 2506              //свободный номер
       MARP
       CPND 
       VMB 
       ANIE 
ITEM 
// сохраняются изменения
MGMT001 TNB CHG TYPE:500  TN:24 0 0 1

MEM AVAIL: (U/P): 2718568    USED U P: 220248 59456    TOT: 2998272 
DISK RECS AVAIL: 768 

// выполняем второй пункт "2186 (цифровой TN 6 12) на номер 2183"
REQ: chg
TYPE: 3902

TN   6 12                 // порт цифровой 6 12
ECHG yes
ITEM key 00 scr 2183      // указываем новый номер 2183
       MARP
       CPND 
       VMB 
       ANIE 
     KEY 
ITEM 

MGMT001 TNB CHG TYPE:3902 TN:6 0 0 12

MEM AVAIL: (U/P): 2718568    USED U P: 220248 59456    TOT: 2998272 
DISK RECS AVAIL: 768 

// выполняем трктий пункт "2506 (аналог TN 24 1) на номер 2186"
REQ: chg
TYPE: 500

TN   24 1
ECHG yes
ITEM dn 2186
       MARP
       CPND 
       VMB 
       ANIE 
ITEM 
// сохраняются изменения
MEM AVAIL: (U/P): 2718568    USED U P: 220248 59456    TOT: 2998272 
DISK RECS AVAIL: 768 
ANALOGUE TELEPHONES    AVAIL:    72    USED:    64    TOT:   136
CLASS TELEPHONES       AVAIL:     0    USED:     0    TOT:     0
WIRELESS TELEPHONES    AVAIL:     0    USED:     0    TOT:     0
WIRELESS VISITORS      AVAIL:     0    USED:     0    TOT:     0
ACD AGENTS             AVAIL:    10    USED:     0    TOT:    10
AST                    AVAIL:     1    USED:     0    TOT:     1
SIP CONVERGED DESKTOPS AVAIL:     0    USED:     0    TOT:     0
SIP CTI TR87           AVAIL:     0    USED:     0    TOT:     0
TNS                    AVAIL:  2156    USED:   344    TOT:  2500
DATA PORTS             AVAIL:  2500    USED:     0    TOT:  2500
PHANTOM PORTS          AVAIL:  2500    USED:     0    TOT:  2500

// отсутствует ли 2506? Да.
REQ: prt
TYPE: dn
TYPE DNB
CUST 0
DN   2506
DATE 
PAGE 
DES 

SCH0881 

NO ACT SINCE NO DATE    
{% endhighlight %}{: .highlight-left }

### 5. Проверим внесенные изменения и внесем номера 2186 и 2183 обратно в группу переадресации. Сохраним. Программы __ld 20__, __ld 18__, __ld 43__

{% highlight javascript %}
// проверим цифровой номер на TN 6 12. Аналоговый выводить не будем. Он также проверяется. 
>ld 20

REQ: prt
TYPE: tn
TYPE TNB
TN   6 12
DATE 
PAGE 
DES  

DES  3902  
TN   006 0 00 12 
TYPE 3902
CDEN 8D
CUST 0 
ERL  0 
FDN  
TGAR 0 
LDN  NO
NCOS 1 
SGRP 0 
RNPG 705 
SCI  0 
SSU  
LNRS 16 
XLST 
SCPW 
SFLT NO
CAC_CIS 3 
CAC_MFC 0
CLS  TLD FBD WTA LPR PUA MTD FNA HTD TDD HFA GRLD 
     MWD LMPN RMMD SMWD AAD IMD XHD IRD NID OLD VCE DRG1
     POD DSX VMD SLKD CCSD SWD LNA CNDA
     CFTA SFD MRD DDV CNID CDCA MSID DAPA BFED RCBD 
     ICDD CDMD MCTD CLBD AUTU
     GPUA DPUA DNDD CFXD ARHD CLTD ASCD 
     CPFA CPTA ABDD CFHA FICD NAID DNAA RDLA BUZZ AGRD MOAD 
     UDI RCC HBTD AHD IPND  DDGA NAMA MIND PRSD NRWD NRCD NROD 
     DRDD EXR0 
     USRD ULAD RTDD RBDD RBHD PGND OCBD FLXD FTTC DNDY DNO3 MCBN 
     FDSD NOVD CDMR MCDD T87D PKCH 
CPND_LANG ROM
RCO  0 
EFD  
HUNT 
EHT  
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
KEY  00 SCR 2183 0     MARP
        ANIE 0 
     01 RNP 
     02 ADL 16  2554
     03 CFW 16  2181
     04 TRN 
     05     
DATE  1 NOV 2017 

**** // выходим из программы __ld 20__ 
// внесем изменения в группе 0011 через программу __ld 18__ 
>ld 18

REQ  chg
TYPE ght
LSNO 0011
SIZE 
WRT  (ADDS: MEM: 0 DISK: 0.0)
STOR 0 2183
WRT  
STOR 2 2186
WRT  
STOR 

MEM AVAIL: (U/P): 2718562    USED U P: 220248 59462    TOT: 2998272 
DISK RECS AVAIL: 768 
**** // выходим из программы __ld 18__ 
//  проверим изменения в группе 0011 
>ld 20

PT0000 
REQ: prt
TYPE: ght
LSNO 0011

GHLN 0011 
GHT  
     
PLDN 0705
DNSZ 4 
STOR 0 2183
STOR 1 2179
STOR 2 2186
STOR 3 2198

// всё в порядке
**** // выходим из программы __ld 20__ 
// если требуется добавить контактную информацию к цифровому аппарату, то используется программа __ld 95__ 
// сохраняем все изменения
> ld 43
.edd
{% endhighlight %}{: .highlight-left }
