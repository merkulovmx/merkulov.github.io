---
layout: post
title: Посмотреть лицензии, версию, релиз Nortel
date: Thu Oct 5 11:10:41 2017 +0200
author: merkulov
tags:
- nortel
- telephony
comments: true
---
### Узнать сколько лицензий и версию Nortel

*В наушниках играет Amantra &mdash; Just For Today*

### 1. Узнать сколько лицензий. Команда __ld 22__
{% highlight javascript %}
ld 22 

REQ slt 
                        Всего        Свободно    Задействовано
ANALOGUE TELEPHONES      128    LEFT    31    USED    97 
CLASS TELEPHONES           0    LEFT     0    USED     0 
DIGITAL TELEPHONES       168    LEFT    19    USED   149 
DECT USERS                 0    LEFT     0    USED     0 
IP USERS                   4    LEFT     4    USED     0 
BASIC IP USERS             0    LEFT     0    USED     0 
DECT VISITOR USER          0    LEFT     0    USED     0 
ACD AGENTS                10    LEFT    10    USED     0 

PCA                        0    LEFT     0    USED     0 
ITG ISDN TRUNKS            0    LEFT     0    USED     0 
H.323 ACCESS PORTS         0    LEFT     0    USED     0 
AST                        1    LEFT     1    USED     0 
SIP CONVERGED DESKTOPS     0    LEFT     0    USED     0 
SIP CTI TR87               0    LEFT     0    USED     0 
RAN CON                    0    LEFT     0    USED     0 
MUS CON                    0    LEFT     0    USED     0 
SURVIVABILITY              0    LEFT     0    USED     0 

TNS                     2500    LEFT  2190    USED   310 
ACDN                     300    LEFT   300    USED     0 
AML                       16    LEFT    15    USED     1 
IDLE_SET_DISPLAY NORTEL 
LTID                       0    LEFT     0    USED     0 
RAN RTE                  500    LEFT   500    USED     0 
ATTENDANT CONSOLES      2500    LEFT  2500    USED     0 
BRI DSL                  150    LEFT   150    USED     0 
MPH DSL                    0    LEFT     0    USED     0 
DATA PORTS              2500    LEFT  2500    USED     0 
PHANTOM PORTS           2500    LEFT  2500    USED     0 
COM001 Ethernet driver: unit 0 is being restarted 

SIP ACCESS PORTS           0    LEFT     0    USED     0 
TRADITIONAL TRUNKS      2500    LEFT  2438    USED    62 
DCH                       80    LEFT    78    USED     2 
TMDI D-CHANNELS           64    LEFT    64    USED     0 
{% endhighlight %}{: .highlight-left }

### 2. Узнать сколько лицензий. Команда __ld 22__
{% highlight javascript %}
ld 22 

REQ iss

CALL SERVER/MAIN CAB 
VERSION 2121 
RELEASE 4 
ISSUE 50 W  + 
IDLE_SET_DISPLAY NORTEL 
{% endhighlight %}{: .highlight-left }
