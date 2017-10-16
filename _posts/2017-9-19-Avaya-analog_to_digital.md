---
layout: post
title: Аналог на цифру Avaya
date: Tue Sep 19 17:00:41 2017 +0200
author: merkulov
tags:
- avaya 
- telephony
comments: true
---
### Требуется поменять местами аналоговый аппарат и цифровой

*!Необходимо отключить аппараты от сети*

### Вариант 1. Подходит только для комбинаций аналог на аналог, цифровой на цифровой
Через ASA(Avaya Site Administration) __не используем режим Start GEDI__ для подключения к станции.
- Нажимаем в левом, боковом меню __Swap Stations__ 
- Вводим номера аппаратов в поля __Extension__ (рис1. 2435 и 2419)

![an image alt text](https://merkulovmx.github.io/images/8image1.jpg "рис. 1")

- Далее. Далее. Соглашаемся с операцией. Готово.(рис 2. Даем согласие)

![an image alt text](https://merkulovmx.github.io/images/8image2.jpg "рис. 2")

При попытке поменять аналоговый на цифровой появится сообщение об ошибке.(рис 3. Ошибка)

![an image alt text](https://merkulovmx.github.io/images/8image3.jpg "рис. 3")

### Вариант 2. Аналог на цифру
Открываем ASA. Подключаемся через режим __Start GEDI__. 
{% highlight javascript %}
// Сохраняем информацию о настройках каждого из аппаратов.
list groups-of-extension 2435 //показать вхождение в группы перехвата и переадресации
change station 2435 //настройки станции
change pickup-group 1 //удаляем номер из группы перехвата
change coverage path 111 //удаляем номер из группы переадресаций
// Аналогичные действия с другим телефонным номером.
list groups-of-extension 2419 //показать вхождение в группы перехвата и переадресации
change station 2419 //настройки станции
change pickup-group 2 //удаляем номер из группы перехвата
change coverage path 222 //удаляем номер из группы переадресаций
// удаляем тел. номера с каждого из портов
remove station 2435
remove station 2419
// создаем вновь, только теперь прописываем на порт, там где ранее стоял предыдущий аппарат, необходимый
add station 2435
add station 2419
// восстанавливаем настройки и членство в группах перехвата и переадресации
change station 2435
change pickup-group 1 
change coverage path 111 
change station 2419 
change pickup-group 2 
change coverage path 222 
save translation
{% endhighlight %}{: .highlight-left }

*!Если выскакивает ошибка при попытке удаления, то значит номер до сих пор имеет членство в какой-то группе*
