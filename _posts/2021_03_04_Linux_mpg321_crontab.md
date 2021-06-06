---
layout: post
title: "Linux: Using crontab and mpg321 to play songs for International Women's Day"
date: Sun Mar 7 22:56:41 2021 +0200
draft: false
author: Maksim Merkulov
tags: 
- crontab
- mpg321
- linux
comments: true
---
## Linux: Using crontab and mpg321 to play songs for International Women's Day 
Task: We have Raspberry Pi with linux and crontab.  
And the main goal is to play music on the 8 of March.  
#### 1. Install mpg321. It is a free command-linemp3 player
```bash
user_name@raspberry_linux: $ sudo apt update
user_name@raspberry_linux: $ sudo apt install mpg321
```
#### 2. Make a directory in __/media__ for an mp3 music
```bash
user_name@raspberry_linux:/media $ sudo mkdir 8_march
user_name@raspberry_linux:/media $ ls -la
drwxr-xr-x  4 root root 4096 march  3 17:03 .
drwxr-xr-x 21 root root 4096 march 18  2016 ..
drwxr-xr-x  2 root root 4096 march  3 17:03 8_march

user_name@raspberry_linux:/media $ sudo chmod 777 8_march
user_name@raspberry_linux:/media $ ls -la
drwxr-xr-x  4 root root 4096 march  3 17:03 .
drwxr-xr-x 21 root root 4096 march 18  2016 ..
drwxrwxrwx  2 root root 4096 march  3 17:03 8_march
```
#### 3. Create a playlist from all 
```bash
user_name@raspberry_linux:/media/8_march $ sudo find ./ -name "*.mp3">>playlist8

user_name@raspberry_linux:/media/8_march $ ls -la
drwxrwxrwx 2 root root     4096 мар  3 17:28 .
drwxr-xr-x 4 root root     4096 мар  3 17:08 ..
-rw-r--r-- 1 user_name   user_name    7657055 march  3 17:05 90.mp3
-rw-r--r-- 1 user_name   user_name         32 march  3 17:28 playlist8

user_name@petvoice02:/media/8_march $ cat playlist8
./90.mp3
```
#### 4. Test how to play the __playlist8__
```bash
user_name@raspberry_linux:/media $ mpg321 --loop N --list /media/8_march/playlist8
High Performance MPEG 1.0/2.0/2.5 Audio Player for Layer 1, 2, and 3.
Version 0.3.2-1 (2012/03/25). Written and copyrights by Joe Drew,
now maintained by Nanakos Chrysostomos and others.
Uses code from various people. See 'README' for more!
THIS SOFTWARE COMES WITH ABSOLUTELY NO WARRANTY! USE AT YOUR OWN RISK!
Title   : ▒▒▒▒▒▒▒ ▒▒▒▒▒▒                 Artist : Flora.At.Ua
Album   :                                Year    :                              
Comment :                                Genre : Ambient
```
#### 5. Kill a play process
```bash
user_name@raspberry_linux:/media $ sudo kill -KILL 5573
[1]+  Killed              mpg321 --loop N --list /media/8_march/playlist8

user_name@raspberry_linux:/media $ ps -aux | grep mpg321
user_name        5759  0.0  1.0   5992  1944 pts/0    S+   12:25   0:00 grep --color=auto mpg321
```
#### 6. Added in a crontab information about new task
| minutes | hours | day | month | day_of_week | user | command |
```bash
user_name@raspberry_linux:~ $ sudo crontab -e
  GNU nano 2.2.6        Файл: /tmp/crontab.zSGOPD/crontab

  00 08 5  3   *     mpg321 --loop N --list /media/8_march/playlist8
  00 02 6  3   *     pkill -TERM mpg321

```
### P.S. Additional information
#### For start mpg321 process in background we can add >/dev/null 2>&1&
```bash
mpg321 --loop N --list /media/8_march/playlist8 >/dev/null 2>&1&
```
#### How it looks in the crontab with change volume
```bash
00 08 5  3   *     mpg321 --loop N --gain 40 --list /media/8_march/playlist8 >/dev/null 2>&1&
```
