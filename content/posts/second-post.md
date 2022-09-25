---
author: Moe Green
title: "Установка загрузчика SLiM в ArchLinux"
date: 2013-01-15
description: Краткий обзор конфигурационного файла. Команды включения, перезагрузки и выключения системы через SLim.
draft: false
tags: [archlinux, slim]
categories: ["linux"]
series: ["Linux"]
---

Установка в ArchLinux стандартная:

{{< highlight powershell >}}
$ sudo pacman -S slim
{{< /highlight >}}

После успешной установки нужно отредактировать файл `/etc/rc.conf`. Если до этого в системе стоял GDM, можно удалить его, а можно закомментировать `gdm` и добавить `slim` в строку:

```powershell
DAEMONS=(@syslog-ng cpufreq laptop-mode @network @net-profiles hal ntpd fam !gdmslim !netfs gpm @crond alsa)
```

Поведение SLiM настраивается через конфигурационный файл `/etc/slim.conf`.
<!--more-->

## Наиболее полезные параметры

- *default_user simone* - Если строка раскомментирована и выставлено имя пользователя в качестве ее значения, то поле `username` заполняется автоматически при входе в SLiM.
- *focus_password no* - Параметр связан с параметром `default_user`. Активируется, если задействован параметр `default_user`. В этом случае фокус автоматически устанавливается в поле `password`.
- *auto_login no* - Автоматический вход в систему пользователя по умолчанию, указанного в параметре `default_user`. Для включения этой опции нужно выставить его значение на `yes`.
- *current_theme ArchLinux-simplyblack* - Тема экрана приветствия SLiM. Все темы располагаются по адресу `/usr/share/slim/themes/`.
- *shutdown_msg The system is halting* - Сообщение, выводимое на экран при выключении компьютера.
- *reboot_msg The system is rebooting* - Сообщение, выводимое на экран при перезагрузке компьютера.
- *welcome_msg Welcome to %host* - Сообщение-приветствие на экране.
- *screenshot_cmd import -window root /slim.png* - Создание скриншота экрана SLiM. Действие команды привязано к клавише F11 клавиатуры.
- *sessions xfce4,icewm,wmaker,blackbox* - Выбор сессии для запуска X-ов. То есть, если в системе стоят несколько WindowManager'ов или DE, как например - Xfce, KDE, GNOME, Openbox и т. д., все они указываются через запятую в качестве значения параметра `sessions`. Первый в списке является значением по-умолчанию. Для выбора нужного графического окружения при входе в SLiM нужно нажать клавишу F1. Параметр связан с командой `login_cmd`. Значение параметра sessions подставляется в качестве переменной %session.

После конфигурирования `/etc/slim.conf` нужно отредактировать файл `~/.xinit` следующим образом:

```powershell
DEFAULT_SESSION="gnome"
case $1 in
  kde)
  exec startkde
  ;;
  xfce4)
  exec startxfce4
  ;;
  openbox)
  exec startopenbox
  ;;
  *)
  exec $DEFAULT_SESSION
;;
esac
```

- _login_cmd exec /bin/bash -login ~/.xinitrc %session_ - Команда, выполняемая при успешном входе в систему (*правильного ввода пары логин-пароль*). Стоит обратить внимание, что если в системе не установлен _bash_ в качестве _shell_'а, то следует заменить значение _/bin/bash_ на тот _shell_, который используется в системе. Например, в FreeBSD нужно выставить _/bin/sh_ вместо _/bin/bash_. Также можно добавить в строку переменную _%theme_ для задания определенной темы SLiM при его старте.
- _halt_cmd /sbin/shutdown -h now_ - Команда выключения компьютера из SLiM.
- _reboot_cmd /sbin/shutdown -r now_ - Команда перезагрузки компьютера из SLiM.
- _console_cmd /usr/bin/xterm -C -fg white -bg black +sb -T "Console login" -e /bin/sh -c "/bin/cat /etc/issue; exec /bin/login"_ - Запуск терминала консоли прямо из SLiM. Можно задать предпочитаемый эмулятор терминала, заменив xterm на свое, например, gnome-terminal, rxvt, aterm, mrxvt, materm, wterm, gnome-multi-terminal, eterm, rxvt-unicode (urxvt), mlterm, mliterm.
- _susliend_cmd /usr/sbin/susliend_ - Команда перевода компьютера в спящий режим из SLiM.
- _numlock on_ - Включение-отключение NumLock клавиатуры при запуске SLiM. Возможные значения параметра - `on` (включена), `off` (выключена).
- _hidecursor false_ - Скрыть-показать курсор при вводе логина-пароля. Возможные значения параметра - `true|false`.

## Полезные команды SLiM

{{< notice tip >}}
Для остановки, перезагрузки, перевода в спящий режим компьютера или запуска эмулятора терминала из SLiM используются команды, представленные ниже.
{{< /notice >}}

Правильный порядок ввода их следующий:

- в поле `username` вводим команду (например, `halt`)
- в поле `password` вводим пароль `root`'а.

Команды:

1. Запуск эмулятора терминала - команда `console`;
2. Выключение компьютера - команда `halt`;
3. Перезагрузка компьютера - команда `reboot`;
4. Перевод компьютера в спящий режим - команда `susliend`;
5. Перейти в консоль - команда `exit`.
