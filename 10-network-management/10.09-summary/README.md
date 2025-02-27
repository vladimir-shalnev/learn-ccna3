<!-- 10.8.3 -->
## Обнаружение устройств с помощью протокола CDP

Cisco Discovery Protocol (CDP) — это проприетарный протокол компании Cisco уровня 2, который служит для сбора информации об устройствах Cisco, использующих один и тот же канал передачи данных. 

Устройство периодически отправляет объявления CDP на подключенные устройства. CDP также можно использовать в качестве сетевого средства обнаружения, чтобы получать информацию о соседних устройствах. Собранная информация помогает построить логическую топологию сети, когда документации нет или он недостаточно детализированная. Протокол CDP помогает проектировать сети, находить и устранять неполадки, а также вносить изменения в оборудование. 

На устройствах Cisco протокол CDP включен по умолчанию. Чтобы проверить состояние CDP и отобразить сведения о нем, введите команду **show cdp**. Чтобы включить CDP сразу для всех поддерживаемых интерфейсов, введите команду **cdp run** в режиме глобальной конфигурации. Чтобы включить CDP на конкретном интерфейсе, введите команду **cdp enable**. Чтобы проверить состояние CDP и просмотреть список соседних устройств, выполните команду **show cdp neighbors** в привилегированном режиме EXEC. Команда **show cdp neighbors** предоставляет полезную информацию о каждом соседнем устройстве CDP, включая идентификаторы устройства, идентификатор порта, список возможностей и платформу. Команда **show cdp interface** отображает интерфейсы устройства, на которых включен протокол CDP.

## Обнаружение устройств с помощью протокола LLDP

Устройства Cisco также поддерживают LLDP (Link Layer Discovery Protocol) — не зависящий от поставщика протокол для обнаружения соседей, подобный CDP. Этот протокол объявляет себя и свои возможности другим устройствам и получает данные от физически подключенных устройств уровня 2. Чтобы включить LLDP для всех интерфейсов сетевого устройства Cisco, введите команду **lldp run** в режиме глобальной конфигурации. Чтобы убедиться, что протокол LLDP включен на устройстве, введите команду **show lldp** в привилегированном режиме EXEC. Если включен протокол LLDP, можно найти соседей определенного устройства с помощью команды **show lldp neighbors**. Если нужна более подробная информация о соседних устройствах, можно воспользоваться командой **show lldp neighbors detail**, которая предоставляет такие сведения, как версии IOS, IP-адреса и функции соседних устройств.

## NTP

Основной источник информации о времени в системе — программные часы маршрутизатора или коммутатора, которые запускаются при загрузке системы. Если время на устройствах не синхронизировано, определить порядок событий и их причину невозможно. 

Настроить дату и время можно вручную или с помощью NTP. Этот протокол синхронизирует по сети настройки времени маршрутизаторов с NTP-сервером. Если в сети реализован протокол NTP, его можно настроить на синхронизацию с частным тактовым генератором или общедоступным сервером NTP в Интернете. 

NTP сети используют иерархическую систему источников времени, где каждый уровень называется слоем (stratum). Для распределения синхронизированной информации о времени по сети используется протокол NTP. Авторитетные источники времени, также называемые устройствами слоя 0, являются высокоточными устройствами хронометража. Устройства слоя 1 подключены напрямую к доверенным источникам времени. Устройства часового слоя 2, например клиенты NTP, синхронизируют свое время с помощью пакетов NTP, которые они получают от серверов часового слоя 1. 

Команда **ntp server** _ip-address_ выдается в режиме глобальной конфигурации для настройки устройства в качестве сервера NTP. Чтобы убедиться, что в качестве источника времени выбран NTP, выполните команду **show clock detail**.  Команды **show ntp associations** и **show ntp status** используют для проверки синхронизации устройства с сервером NTP.

## SNMP

Протокол SNMP разработали, чтобы администраторы управляли узлами в сети IP: серверами, рабочими станциями, маршрутизаторами, коммутаторами и устройствами информационной безопасности. SNMP — протокол уровня приложений, предоставляющий формат сообщения для обмена данными между диспетчерами и агентами. 

Система SNMP состоит из трех элементов: диспетчер SNMP, агенты SNMP и MIB. Для настройки SNMP на сетевом устройстве сначала необходимо указать отношения между диспетчером и агентом. Менеджер SNMP — часть NMS. Диспетчер SNMP может собирать данные от агента SNMP с помощью запроса get и изменять настройки на агенте с помощью запроса set. Агенты SNMP могут пересылать информацию непосредственно в диспетчер сети, используя ловушки (пакеты trap). Агент SNMP отвечает на GetRequest-PDU диспетчера SNMP (для получения переменной MIB) и SetRequest-PDU (для установки переменной MIB). NMS периодически использует запрос get для опроса агентов SNMP. Приложение для управления сетями может собирать информацию для мониторинга транспортной нагрузки и проверять настройки управляемых устройств.

SNMPv1, SNMPv2c и SNMPv3 — все это версии SNMP. SNMPv1 — устаревшее решение. В SNMPv1 и SNMPv2c используется модель безопасности на основе сообществ (community). Сообщество диспетчеров, имеющих доступ к базе MIB-агента, определяется строкой сообщества. Версия SNMPv2c предусматривает механизм массового извлечения записей и более подробные уведомления об ошибках. SNMPv3 предусматривает как модели безопасности, так и уровни безопасности. 

Строки сообщества SNMP доступны только для чтения (ro) и для чтения и записи (rw). С их помощью проверяют подлинность доступа к объектам MIB. Переменные в MIB организованы иерархически. Переменные MIB позволяют программному обеспечению наблюдать за сетевым устройством и контролировать его. OID представляют собой уникальные управляемые идентификаторы в иерархии MIB. Эта служебная программа дает некоторое представление о базовых механизмах работы SNMP. Навигатор Cisco SNMP на веб-сайте [http://www.cisco.com](http://www.cisco.com) позволяет администратору сети просматривать сведения о конкретном идентификаторе OID.

## Syslog

Самый распространенный способ получать системные сообщения — протокол syslog. Он использует 514 порт UDP, что позволяет сетевым устройствам отправлять системные сообщения по сети на серверы syslog. 

Основные функции службы ведения журнала syslog: сбор сведений журнала для мониторинга и устранения неполадок, выбор типа регистрируемой информации и определение назначения захваченных сообщений системного журнала. Пункты назначения для сообщений системного журнала включают буфер (RAM внутри маршрутизатора или коммутатора), консольную линию, линию терминала и сервер. В этой таблице показаны уровни системного журнала:

| **Название уровня серьезности** | **Уровень серьезности** | **Описание** |
| --- | --- | --- |
| Чрезвычайная ситуация | Уровень 0 | Систему нельзя использовать |
| Предупреждение | Уровень 1 | Требуется принять немедленные меры |
| Критический | Уровень 2 | Критическое состояние |
| Ошибка | Уровень 3 | Состояние ошибки |
| Предупреждение | Уровень 4 | Состояние предупреждения |
| Уведомление | Уровень 5 | Нормальное, но требующее внимания состояние |
| Информационный | Уровень 6 | Информационное сообщение |
| Отладка | Уровень 7 | Сообщение отладки |

Объекты syslog (syslog facilities) — идентификаторы сервисов, которые определяют и классифицируют данные о состоянии системы для отчетов об ошибках и событиях. Общие средства сообщений системного журнала включают: IP, протокол OSPF, операционную систему SYS, IPsec и IF. Формат сообщений системного журнала в программном обеспечении Cisco IOS по умолчанию: %facility-severity-MNEMONIC: описание. Команда **service timestamps log datetime** позволяет принудительно отображать дату и время для зарегистрированных событий.

## Поддержка файловой системы маршрутизатора и коммутатора

Файловая система Cisco IOS (IFS) позволяет администраторам перемещаться по различным каталогам, отображать в них списки файлов, а также создавать вложенные каталоги во флеш-памяти или на диске. 

Команда **show file systems command** показывает все доступные файловые системы на маршрутизаторе Cisco. Используйте команду **dir** для отображения каталога bootflash. Используйте команду change directory **cd** для просмотра содержимого NVRAM. Используйте текущую команду рабочего каталога **pwd** , чтобы вы просматривали текущий каталог. Используйте **show file systems** эту команду для просмотра файловых систем на коммутаторе Catalyst или маршрутизаторе Cisco. 

Файлы конфигурации можно сохранить в текстовом файле, используя программу Tera Term. Конфигурацию можно скопировать из файла, а затем напрямую вставить на устройство. Файлы конфигурации можно хранить на сервере TFTP или на USB-накопителе. Чтобы сохранить текущую конфигурацию или конфигурацию начальной загрузки на TFTP-сервер, используйте команду **copy running-config tftp** или **copy startup-config tftp**. Используйте команду **dir** для отображения содержимого USB-накопителя. Используйте команду **copy run usbflash0:/**, чтобы скопировать файл конфигурации на USB-накопитель. Используйте команду **dir**, чтобы просмотреть файл на USB-накопителе Используйте команду **more** для отображения содержимого USB-накопителя. Если пароль зашифрован (как, например, секретные пароли для входа в режим настройки), после восстановления его необходимо заменить.

## Управление образами IOS

Образы программного обеспечения Cisco IOS и файлы конфигурации могут храниться на центральном сервере TFTP для управления количеством образов IOS и их редакциями, а также файлами конфигурации, которые необходимо поддерживать. Выберите файл образа Cisco IOS, подходящий к оборудованию и обладающий нужным функционалом. Загрузите файл с сайта cisco.com и отправьте его на TFTP-сервер. Отправьте эхо-запрос на TFTP-сервер. Проверьте количество свободной флеш-памяти. это можно сделать с помощью команды **show flash:**. Если для хранения нового образа IOS достаточно памяти, скопируйте новый образ IOS во флэш-память. Чтобы обновить до скопированного образа IOS после его сохранения, укажите маршрутизатору использовать новый образ во время загрузки с помощью команды **boot system**. Сохраните конфигурацию. Перезагрузите маршрутизатор, чтобы он загрузился с новым образом. После загрузки маршрутизатора убедитесь, что новый образ загрузился. Используйте для этого команду **show version**

<!-- 10.8.4 -->
<!-- quiz -->
