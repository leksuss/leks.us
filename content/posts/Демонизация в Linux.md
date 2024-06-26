---
title: Демонизация в Linux
date: 2023-07-25T23:48:00+03:00
draft: false
---
Прежде всего, давайте определимся, что такое демонизация. Представьте, что у вас есть простой python скрипт, который вы запускаете на каком-то удаленном сервере подключившись к нему через ssh. Если вы закроете ssh соединение, скрипт перестанет работать.

Как же исправить эту ситуацию? Что ж, вы можете использовать терминальный менеджер, типа `tmux`. Он создает множество терминальных окон, и вы можете переключаться между ними. И переключение не останавливает работу работающих процессов в предыдущем терминальном окне. Также есть программа `screen` с тем же функционалом.

Но что произойдет, если ваш сервер будет перезагружен? К сожалению, ваш скрипт не запустится. И вот теперь к нам на помощь приходит демонизация! Вы можете демонизировать любой скрипт, любой процесс, любую программу. И она будет запускаться автоматически после запуска сервера, она сможет автоматически перезапускаться в случае падения из-за какой-либо ошибки, она может работать в бэкграунде, и вы сможете читать её логи в приятном формате! Звучит отлично, не так ли?

`systemd`  - это программа-арбитр для других программ. Иначе говоря, это программа, которая запускает другие программы и контролирует их статус. Программа, которая может быть запущена таким образом, называется демоном. Когда вы делаете из обычной программы демона, вы демонезируете эту программу. 

Сделать демона из вашего скрипта (или любой другой программы) очень просто. Просто следуйте шагам ниже. Имейте ввиду, что все шаги должны быть сделаны с привилегиями суперпользователя.

1. Создайте файл в папке `/etc/systemd/system` с названием вашего скрипта  и расширением `.service`. Например, `my_cool_script.service` с таким текстом внутри:
```ini
[Service]
ExecStart=/path/to/python3 /path/to/script/my_cool_script.py
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Перезагрузите сервис `systemd` чтобы подтянуть изменения. Для управления   `systemd`, которая называется`systemctl`. Поэтому вам нужно выполнить команду перезагрузки таким образом:
```bash
systemctl daemon-reload
```
3. Запустите ваш скрипт-демон:
```bash
systemctl start my_cool_script.service
```
4. Если вам нужно, чтобы скрипт запускался автоматически при загрузке, запустите эту команду. Она создаст символическую ссылку в автозагрузке и включит эту возможность:
```bash
systemctl enable my_cool_script.service
```

Собственно, это всё. Давайте объясню, что означает каждая линия в файле `my_cool_script.service`. `[Service]` - это название секции настроек, которую нужно оставить как есть. Далее `ExecStart` хранит команду, которую `systemd` должен преобразовать в демона.  `Restart` - этот параметр задает возможность перезапуска демона в случае если демон упал. `[Install]` - это секция для определения настроек при старте демонизированной программы. `WantedBy=multi-user.target` означает, что ваш скрипт должен запускаться после того, как сервер загрузится.

В одной из следующих статей я расскажу о более сложных скриптах, например, демонизации запуска Django.
