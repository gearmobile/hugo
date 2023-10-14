+++
authors = ["Muso Verda"]
title = "Ubuntu 22.04 - KVM virtualization"
date = "2023-09-29"
description = "Ubuntu 22.04.3 - после установки столкнулся с проблемой при выключении системы с ошибкой `kvm: exiting hardware virtualization`"
tags = [
    "ubuntu",
    "kvm",
]
categories = [
    "linux"
]
+++

## Kvm: exiting hardware virtualization

Итак, система Ubuntu 22.04.3 LTS свежеустановленная, все обновления установлены. Но при выключении системы в логах возникает ошибка `kvm: exiting hardware virtualization`. Система зависает и выключить ее можно лишь при помощи hard shutdown.

Перепробовал разные советы и решения по этой теме в Сети, но эффективным оказалось следующее.

1. Открываем терминал

2. В терминале вводим следующую строку:
{{< highlight bash >}}
EDITOR=gedit sudoedit /etc/default/grub
{{< /highlight >}}

3. Откроются настройки GRUB в редакторе gedit - в файле находим строку:
{{< highlight text >}}
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi"
{{< /highlight >}}
... и меняем ее на строку:
{{< highlight text >}}
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi=force"
{{< /highlight >}}
... то есть, по факту - добавляем для ключа `acpi` значение - `force`.

4. Сохраняем изменения в конфигурационном файле GRUB.

5. В терминале делаем обновления загрузчика GRUB командой:
{{< highlight bash >}}
sudo update-grub
{{< /highlight >}}
