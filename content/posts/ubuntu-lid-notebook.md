+++ 
draft = false
date = 2022-09-25T19:39:33+03:00
title = "Ubuntu - проблема с KVM Switch"
description = ""
slug = ""
authors = ["Moe Green"]
tags = ["ubuntu", "kvm switch", "lid switch"]
categories = []
externalLink = ""
series = ["Linux", "Ubuntu"]
+++

Суть вопроса - имеется личный ноутбук с Ubuntu Gnome 22.04 и есть рабочий ноутбук с Windows 10. Ради удобства работы - был приобретен KVM Switch - для подключения обоих ноутбуков - к одному монитору, клавиатуре и мыше; и удобного и быстрого переключения между ноутами.

Подключение было выполнено успешно. Однако, возникла небольшая проблема - если переключиться с работающего ноутбука Ubuntu на ноутбук с Windows, а затем - выполнить переход обратно - с ноутбука Windows на ноутбук Ubuntu, то в этом случае возникала следующая ситуация.

Ноутбук Ubuntu - к моменту переключения на него обратно - успевал "заснуть"; и заставить его "проснуться" можно было - только открыв крышку ноута. Весьма неудобно и теряется смысл существования kvm switch'а.

После некоторого изыскания и помощи на стороне reddit - решение было найдено. Оригинал помощи находится здесь - [How to Change Lid Close Behavior in Ubuntu 20.04](https://ubuntuhandbook.org/index.php/2020/05/lid-close-behavior-ubuntu-20-04/)
<!-- more -->

### Шаги исправления просты

В терминале нужно открыть для редактирования файл `logind.conf`:

```bash
sudo nano /etc/systemd/logind.conf
```

В открытом файле - найти строку `HandleLidSwitch`; по-умолчанию она будет иметь такой вид:

```text
...
#HandleLidSwitch=suspend
...
```

Нужно ее раскомментировать - убрать символ `#`; и изменить значение на `ignore`. Итоговый вариант файла будет выглядеть так:

```text
...
[Login]
...
#UserStopDelaySec=10
#HandlePowerKey=poweroff
#HandleSuspendKey=suspend
#HandleHibernateKey=hibernate
HandleLidSwitch=ignore
#HandleLidSwitchExternalPower=suspend
#HandleLidSwitchDocked=ignore
#HandleRebootKey=reboot
...
```

После этого - достаточно выполнить команду:

```bash
sudo systemctl restart systemd-logind.service
```