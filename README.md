# Установка на Intel-PC
## Подбор и проверка совместимости железа
* **Процессор**: обязательно Intel (существуют варианты установки на AMD, но это по большей части костыль и того не стоит)
* **Видеокарта**: на данный момент можно "завести" любую видеокарту (есть исключения¹).
* **Материнская плата**: частично бывают проблемными материнские платы на X\*-чипсетах.
* **Аудио**: работает практически всё, но бывают исключения.
* **Сеть**: заводимо всё, кроме китайских ноунейм LAN-карт.
* **Bluetooth и Wi-Fi**: смотреть [таблицу](https://docs.google.com/spreadsheets/d/1Yxlvo-vK_zupMiR1Ua_NZ1X0j6BSXoICCEgB7wvAQsM/edit#gid=0).
## Настройка BIOS Legacy и BIOS UEFI
* **CSM**: На современных платах и видеокартах при использовании исключительно UEFI-загрузки рекомедуется его выключать.
* **Secure Boot**: Other OS (UEFI).
* **SATA**: Обязательно поставить режим AHCI.
* **HPET**: Включить.
* **Fast Boot** и **Hardware Fast Boot**: Выключить.
* **Above 4G Decoding**: Включить.
* Отключаем USB 3.0 и 3.1 во избежание проблем во время установки.
* Отключаем Serial-порты и подобные неиспользуемые интерфейсы.
* Отключаем Bluetooth.
### Примечания:
* На сборках с двумя CPU требуется поставить двухядерный режим на обоих CPU.
* Видеокарта должна быть на время установки одна и установлена в первый слот.
* Отключить все мониторы, кроме основного.

## Clover
**Clover** - это загрузчик, который позволяет на обычном компьютере запустить **macOS**. **Apple** этого делать не разрешает, в первую очередь мотивируя тем, что “мы не можем обеспечить работоспособность на компьютерах, произведенными не компанией **Apple**”. Поэтому ставим систему на свой страх и риск.

## config.plist
Этот файл используется для настройки загрузчика **Clover**. Он - основа всего, то, что заставить вашу систему работать правильно.
**Clover** умеет генерировать файл конфигурации (далее - **конфиг**), основанный на вашем "железе" самостоятельно, но как вы знаете, нет ничего идеального. Поэтому у пользователя есть возможность менять параметры "конфига" напрямую в файле или на ходу в настройках **Clover'a**. Файл написан на языке **XML**, что существенно упрощает работу с ним, так как этот язык является user-friendly. Файл должен находится в **EFI/CLOVER**. Его можно редактировать как с помощью простых редакторов (`Notepad++`, `Sublime Text`, `Atom`, `nano`, `vim`), так и с более специализированными под это дело (**PlistEdit**, встроенный редактор в **Xcode**). Также с недавнего времени появился веб-редактор, заточенный под "конфиг" – [**CloverCloudEditor**](http://cloudclovereditor.altervista.org/cce/index.php).

## Создание загрузочной USB-флешки
Самым лучшим и правильным способом по мнению русского хак-коммьюнити является установка чистого образа из App Store. С помощью программы **BootDiskUtility** (в сокращении - BDU) и образа `.hfs` можно получить наиболее чистую систему. 
Естественно, существует не один способ установки Хакинтоша, но сейчас мы рассмотрим наиболее популярный. 
1. Скачиваем **BootDiskUtility** [отсюда](http://cvad-mac.narod.ru/index/bootdiskutility_exe/0-5).
2. Распаковываем утилиту в любую папку. 
3. Скачиваем образ **macOS** [отсюда](https://mac-ru.net/viewtopic.php?t=41), [отсюда](https://nnmclub.to/forum/viewtopic.php?t=1069291) или с [магнет-ссылки](magnet:?dn=BDUOSXDISTR&xt=urn:btih:64125b9f1387632e1b35b8da27eba422f9821d43). 
4. Распаковываем образ из архива.
5. Открываем **BootDiskUtility**, заходим в секцию настроек, нажимаем **Check Now**. Это проверит последнюю версию **Clover** и выберет её в качестве версии для записи на USB. 
6. Выбираем своё USB-устройство, нажимаем **Format Disk**. Дожидаемся записи бутлоадера на USB. 
> На этом шаге у вас уже должен быть скачан образ **macOS** в виде `5.hfs`.  
7. Нажимаем на значок `+` рядом с названием USB. Если вы ничего не меняли в настройках, то у вас появится два раздела, один из которых будет иметь название `CLOVER`, а другой `NONAME`.
8. Проверяем наличие `CLOVER`, в разделе clover на USB если пусто. в настройках выбераем локальный файл кловера ссылка на файл 

БУДЬТЕ ДОБРЫ ВНЕСИТЕ ИЗМЕНЕНИЯ САМИ ДОБАВИВ ССЫЛКУ НА СКАЧИВАНИЕ КЛАВЕРА

СПАСИБО 

9. Выбираем `Part2`, который имеет название `NONAME`. Нажимаем кнопку **Restore Partition** и указываем прежде скачанный `5.hfs`.
 
Начнется запись образа на USB. 

Теперь у вас есть готовая USB с образом **macOS**. Вы совершили свой первый шаг к установке Хакинтоша.

## Установка образа на HDD/SSD
1. Для начала рекомендуется внести минимальные правки в config.plist, прописав в `Boot/Arguments` `-v debug=0x100 keepsyms=1`, а если ставите High Sierra, то и добавить [патч на показ паники](https://4pda.ru/forum/index.php?act=findpost&pid=62112835&anchor=Spoil-62112835-1).
2. Положите kext-ы на сеть, звук и видео. 
> В случае High Sierra и новее не забудьте скопировать `ApfsDriverLoader-64.efi` из папки drivers-Off в drivers64 или drivers64UEFI в зависимости от типа загрузки.
3. Загружаемся с готовой USB в Clover и выбираем пункт **Boot macOS from OS X Base System**. 
> При первом запуске (если он успешен), вас встретит дружелюбное окно с выбором языка.
4. Выбираем нужный язык, нажимаем **Далее**
> Для macOS до версии Sierra (10.12) включительно диск нужно форматировать в файловую систему HFS+ (OS X Extended (журналируемая)), 
  High Sierra - HFS+ для жёсткого диска и APFS для SSD,
  Mojave - APFS для любого типа носителя.
  
> При использовании APFS убедитесь в наличии драйвера [`ApfsDriverLoader.efi`](https://github.com/acidanthera/AppleSupportPkg/releases) в папке drivers64UEFI/drivers64 в зависимости от типа загрузки.

> Если вы уже отформатировали диск, то переходим к шагу 8.
5. Дальше нам нужно отформатировать диск. Наводим на верхнюю панель, в ней нажимаем **Утилиты - Дисковая утилита**. 
6. Выбираем нужный нам раздел, нажимаем **Стереть**. Вводим название, файловую систему и таблицу **GUID**.
7. Дожидаемся окончания форматирования и закрываем утилиту.
8. Нажимаем далее и выбираем свежеотформатированный диск.
9. После завершения установки вам надо будет перезагрузить систему, но на этот раз в Clover надо выбирать **Boot macOS from %названиедиска%**
10. Как только произойдет загрузка системы, вам предложат настроить основные компоненты.
После окончания настройки у вас будет чистая система **macOS**, но, к сожалению, загрузка возможно только с USB. Чтобы это исправить нужно установить **Clover** в _EFI_ раздел, процесс установки которого я опишу ниже.

## Установка Clover на EFI раздел
* В обоих случаях нужно скачать .pkg-установщик [по ссылке](https://sourceforge.net/projects/cloverefiboot/files/latest/download). Всё это будет делаться из уже запущенной macOS.
* Выбираете системный диск через кнопку `Изменить размещение установки` и переходите в раздел `Настроить`, где и конфигурируете установку загрузчика.
* Ставите галочку `Установить Clover на EFI раздел (ESP)`
* Контрольную панель Clover устанавливайте по желанию.
### UEFI
* В разделе `Загрузчик` выбираете `Не обновлять сектора MBR и PBR`.
* В разделе `Drivers64UEFI` дополнительно выбираете `AptioMemoryFix-64` и `ApfsDriverLoader-64`.
### Legacy
* В разделе `Загрузчик` выбираете `Установить boot0af на MBR` (типовая настройка, в экзотических случаях изучайте форумы).
* В разделе `Drivers64` дополнительно выбираете `ApfsDriverLoader-64` (если вдруг устанавливаете High Sierra на SSD).
* `Установить системные RC-скрипты на основной раздел`
> Про назначение других драйверов можно почитать в "Клевере цвета хаки".

После установки можно и нужно скопировать `config.plist`, папки `kexts` и `ACPI\patched` (если использовался) с загрузочной флешки в соответствующие места на появившемся разделе EFI системного диска.

## Видео
В данный момент большая часть поддерживаемых системой видеокарт заводятся через плагин [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases) к [Lilu.kext](https://github.com/acidanthera/Lilu/releases). Инструкции по его использованию к конкретным видеокартам – ниже по этому тексту. 
## Звук
**На данный момент существует два основных метода "завода" звука: AppleALC и VoodooHDA.**
### AppleALC:  
> Этот метод является динамическим патчингом нативного AppleHDA.
* Само расширение сделано для Realtek ALC кодеков, которые сейчас являются самыми популярными среди производителей материнских плат. 
* Также работает для "завода" звука через HDMI на картах Intel, AMD и NVIDIA.
* Список поддерживаемых кодеков доступен [здесь](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs).

**Установка:**
* Скачиваем [Lilu](https://github.com/acidanthera/Lilu/releases) и сам [AppleALC](https://github.com/acidanthera/AppleALC/releases);
* Кладём оба кекста в **CLOVER/EFI/kexts/Other**.
* Выключаем `FixHDA`, `AddHDMI`, `UseIntelHDMI` в `config.plist`;
* В `config.plist` в **строке** `Devices/Audio/Inject` пишем NO;
* В `Boot/Arguments` добавляем `alcid=X`, где X - номер layout-а, который идёт вместе с кодеком в таблице;
* Перезагружаемся.
> Примечание: если звук так и не появился, то пробуем другой layout. 

### VoodooHDA: 
* Сможет "завести" почти любой аудиокодек, но настройка самого расширения иногда очень проблематична. 
**Установка:**
* Скачиваем сам [кекст](https://sourceforge.net/projects/voodoohda/files/latest/download).
* Кладём кекст в `/EFI/CLOVER/kexts/Other`.
* Перезагружаемся.
* Про дополнительную настройку VoodooHDA можно почитать [здесь](https://applelife.ru/posts/589135).

## Wi-Fi
### Atheros AR****
* Скачиваем [Injector](https://github.com/black-dragon74/ATH9KFixup/releases) и ставим в папку с кекстам и в Clover (если сеть отвалится после перезагрузке переместить его в System/Library/Extensions)
* Скачиваем [ATH9KFixup](https://github.com/black-dragon74/ATH9KFixup/releases/download/1.2.0/ATH9KInjector.zip) и перемещаем в папку с кекстами в Clover.
(Опционально) * Для AR9485 и AR9565 нужно будет дополнительно прописать аргумент -ath9485 и -ath9565, соответственно.
* Перезагружаемся.
> Поддерживаемые модели: AR946X (AR9462 & AR9463), AR9485 и  AR9565.
### Broadcom
> Смотреть [таблицу](http://goo.gl/doHbns).
### Intel 
* Ни один адаптер от Intel не заводим на данный момент. Но ведется разработка [кекста](https://github.com/ammulder/AppleIntelWiFiMVM) под все это дело.
### Qualcomm Atheros QCA****
* Ни один адаптер серии QCA не заводим на данный момент. 

## LAN
### Realtek
* Ставим [кекст](https://github.com/RehabMan/OS-X-Realtek-Network)
* Перезагружаемся.
> Поддерживает все RTL81**.
### Intel 
* Ставим [кекст](https://github.com/Mieze/IntelMausiEthernet)
* Перезагружаемся.
> Поддерживает большинство LAN от Intel (включая новые I217*, I218* и I219*)
### Atheros
* Удаляем ALXEthernet.kext в S/L/E.
* Удаляем соответсвующий сетевой интерфейс в настройках.
* Ставим [кекст](https://github.com/Mieze/AtherosE2200Ethernet/)
* Перезагружаемся.
> Поддерживает AR816x, AR817x, Killer E220x, Killer E2400 и Killer E2500.
### Broadcom
* Используем FakePCIID.

# Полезные ссылки 
## Главное 
* Дистрибутивы — http://osxpc.ru/category/downloads/osx/
* Кексты 1 – https://www.applelife.ru/threads/2942933
* Кексты 2 — http://osxpc.ru/downloads/k/kexts/
* Запаска /S/L/E — http://osxpc.ru/zapaska/
* Clover — https://sourceforge.net/projects/cloverefiboot/files/latest/download
* Клевер цвета хаки 4542 — https://i.applelife.ru/2018/06/431578_Klever_cveta_xaki_4542.pdf
* "Чистый" config.plist для редактирования под себя – https://sourceforge.net/p/cloverefiboot/code/HEAD/tree/CloverPackage/CloverV2/EFI/CLOVER/config-sample.plist 
* Настройка BIOS (общее) — http://osxpc.ru/faq/settings_bios/
### Обсуждение 
* Clover — https://applelife.ru/threads/42089
* Mojave – https://applelife.ru/threads/2942975
* High Sierra – https://applelife.ru/threads/2210706
* Sierra – https://applelife.ru/threads/2929575
* El Capitan — https://applelife.ru/threads/517495
* Yosemite — https://applelife.ru/threads/41758

## Графика
### Intel
* Актуальный мануал – https://applelife.ru/threads/1289648
* Intel Quick Sync – https://applelife.ru/threads/817923/
* Skylake HD – http://osxpc.ru/zavod/gpu/intel/skylake/
* Broadwell HD – http://osxpc.ru/zavod/gpu/intel/broadwell/
* Haswell HD – http://osxpc.ru/zavod/gpu/intel/haswell/
* HD4000 — http://osxpc.ru/zavod/gpu/intel/hd4000/
* HD3000 — http://osxpc.ru/zavod/gpu/intel/hd3000/
* GMA X3100 — https://applelife.ru/threads/36617
* GMA 950 — https://applelife.ru/threads/22726
### Radeon
* FAQ по WhateverGreen – https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.Radeon.ru.md
* AMD/ATI HD4xx0, HD5xx0; AMD HD6xx0, HD7xx0, R5, R7, R9, RX, Vega — https://applelife.ru/threads/28890
* Патч фреймбуфера — http://osxpc.ru/zavod/gpu/ati/patch/
### Nvidia
* Fermi – https://applelife.ru/threads/27607
* Kepler — https://applelife.ru/threads/37131
* Maxwell/Pascal – https://applelife.ru/threads/1546195
* Web-драйвера под конкретные версии macOS – http://www.macvidcards.com/drivers.html

## Звук
### AppleALC:
* Релизы (скачивание) – http://github.com/acidanthera/AppleALC/releases
* Поддерживаемые кодеки – http://github.com/acidanthera/AppleALC/wiki/Supported-codecs
* Видео по установке – https://www.youtube.com/watch?v=xbLn_OkVuKY

* AppleHDA (Если нет поддержки со стороны AppleALC) — http://osxpc.ru/kexts/audio/applehda/
* VoodooHDA (Полностью рабочий аналог AppleHDA) — https://sourceforge.net/projects/voodoohda/files/latest/download
* Toleda HDMI — http://tonymacx86.com/threads/audio-hdmi-audio-applehda-guide.143760/
### Обсуждение:
* AppleALC — https://applelife.ru/threads/171672
* VoodooHDA — https://applelife.ru/threads/18413

## СЕТЬ
### RJ-45 (LAN):
* Полный список + обсуждение — https://applelife.ru/forums/setevye-karty.40/
* Самые популярные — http://osxpc.ru/category/kexts/internet/
### Wi-Fi (WLAN):
* Новая таблица (NGFF+PCI-E) — https://pp.vk.me/c630723/v630723383/271b8/8P0Uz2vtNIE.jpg
* Старая таблица (только PCI-e) — https://vk.com/doc-12954845_437058293
* Также стоит взглянуть — https://github.com/toleda/wireless_half-mini

## PS/2, I2C (Клавиатура, мышь, тачпад)
* Десктопный вариант — http://osxpc.ru/kexts/ps2/vdps2/
* ELAN — http://osxpc.ru/kexts/ps2/elan/
* Synaptic и прочие — http://osxpc.ru/kexts/ps2/synaptic/

## Аккумулятор
* ACPIBatteryManager.kext — https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads/
* Инструкция по патчингу DSDT – https://www.tonymacx86.com/threads/116102
* Патчи для DSDT (репозиторий) — http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master

## USB
* USBInjectAll.kext – https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/
* Большой мануал по заводу USB – https://applelife.ru/posts/550233
* Создание USBLegacy.kext – https://applelife.ru/posts/537459

## СИСТЕМА И ЮЗАБЕЛЬНОСТЬ
* Завод MacAppStore и iCloud; iMessage и FaceTime; HandOff и Continuity — https://applelife.ru/posts/727913
* Работа с ACPI (DSDT, SSDT) — http://osxpc.ru/faq/acpi-manual/
* Регулировка яркости в ноутбуках — http://osxpc.ru/faq/brightness-laptop/
* Регулировка яркости клавишами Fn — http://osxpc.ru/faq/backlight-fn/
* Ошибка boot0: error — http://osxpc.ru/faq/boot0-error/
