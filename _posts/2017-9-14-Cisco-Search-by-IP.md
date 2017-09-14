---
layout: post
title: Найти интерфейс на Cisco по IP
date: Thu Sep 14 17:17:41 2017 +0200
author: merkulov
tags:
- cisco 
- network
comments: true
---
###  Найти интерфейс на Cisco и изменить Vlan 

*Есть главный шеститонник и разнообразные другие Cisco*

Запускаем консоль __win+R__. Команда __cmd__. __Enter__.

Горячие кнопки:

- __Tab__ &mdash; дополнить ввод команды

### 1. Подключаемся к Шеститоннику 

{% highlight javascript %}
__telnet XX.0.*.1__
#sh arp | __IP-адрес абонента__ //Узнать MAC-адрес
*Internet __IP-адрес абонента__  0 MAC-адрес ARPA Vlan-1*
#sh mac-address-table address __MAC-адрес__ //Находим нужный нам порт, в нашем случае это Gi1/9
|vlan|mac address|type|learn|age|ports
Vlan-1|MAC-адрес|dynamic|Yes|0|Gi1/9
#sh int Gi1/9 //в появившемся море информации нам интересна строка __Description: Trunk to XX.0.*.17__ 
//Это и есть наша Cisco.
#ex
{% endhighlight %}{: .highlight-left }

### 2. Подключаемся к Cisco

{% highlight javascript %}
__telnet XX.0.*.17__
#sh mac-address-table address __MAC-адрес__ //Находим нужный нам интерфейс, в нашем случае это Fa0/10
vlan|mac address|type|ports
Vlan-1|MAC-адрес|dynamic|Fa0/10
#sh vl br //посмотреть vlan и интерфейсы
#conf t //перейти в режим конфигурации
 #int Fa0/10 //выбрать интерфейс
 #switchport access vlan 431 //занести в какой надо vlan
 #switchport voice vlan 14 //если надо, то и в голосовой
 #ex
#end
#wr mem
#ex
#ex
{% endhighlight %}{: .highlight-left }

*Лучше использовать консоль с возможностью лога, чтобы обнаружить проблемы или внесенные ошибки!*
