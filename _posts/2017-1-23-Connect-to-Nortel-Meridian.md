---
layout: post
title: Как подключиться к станции Nortel Meridian
description: 
tags: [nortel]
---

### Как подключиться к станции Nortel Meridian

Nortel Meridian - это телефонная станция. (старая, как советские времена)

Для удаленного подключения к ней, необходимо знать IP-адрес станции. 

Чтобы подключиться будем использовать программу [SecureCRT](https://www.vandyke.com/download/securecrt/download.html)

После установки запустите SecureCRT. Будем устанавливать соединение по протоколу RLogin:

1. Запустить SecureCRT.
2. Создать новую сессию.
  -  Правой кнопкой нажать по __Sessions__ 
  - Выбрать __New Session__ (рис. 1)
  ![an image alt text](http://lepotuli.ru/merkulov/images/1image1.jpg "рис. 1")
  - Вызвать список __Protocol__ и выбрать __Rlogin__ (рис. 2)
  ![an image alt text](http://lepotuli.ru/merkulov/images/1image2.JPG "рис. 2")
  - Нажать Далее и указать данные в поля __Hostname__(IP-адрес) и выбрать __Username__(например, CPSID1111) (рис. 3)
  ![an image alt text](http://lepotuli.ru/merkulov/images/1image3.JPG "рис. 3")
3. Подключение создано.

Для запуска подключения, необходимо сделать двойной клик.

При успешном создании, появится вкладка с подключением. Подключение может какое-то время не реагировать на действия, но затем потребует ввода логина и пароля.

Зная пароль администратора. Ввод логина и пароля, стоит завершать нажатием кнопки Enter

```javascript
logi Имя_пользователя
Ввести пароль
```

При успешной аутентификации появится сообщение, что вход выполнен (рис. 4) 

![an image alt text](http://lepotuli.ru/merkulov/images/1image4.JPG "рис. 4")

Иначе ошибка входа. (рис. 5)

![an image alt text](http://lepotuli.ru/merkulov/images/1image5.JPG "рис. 5")

#### Чтобы завершить работу со станцией нужно набрать команду __logo__ и затем только нажать __Disconnect__. (рис. 6)

![an image alt text](http://lepotuli.ru/merkulov/images/1image6.JPG "рис. 6")

