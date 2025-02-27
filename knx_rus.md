# KNX

**Функционал поддерживается только для [Pro](/sls_pro_rus.md) версии.**

Шлюз поддерживает работу по протоколу KNX начиная с версии *2022.01.07d10*.

Шлюз работает в режиме монитора шины и все обнаруженные групповые объекты сохраняются во внутренней базе данных. На данный момент она рассчитана на 1000 адресов, возможно увеличение количества в дальнейшем.

## Варианты подключения
1) KNX TP (медная шина) - требует специализированного модуля сопряжения (в разработке)
2) KNXnetIP/Tunneling (unicast) - подключение к KNX IP Interface / Router
3) KNXnetIP Routing (multicast) - в данный момент не планируется, т.к. не гарантируется доставка пакетов

## Программирование через ETS
Планируется добавить функционал для программирования KNX через шлюз, т.е. использование шлюза как полноценного IP Interface.

При этом объекты находящиеся в базе будут отдаваться на запрос чтения без физического запроса в шину TP (кэширование), что значительно повышает скорость запуска визуализаций.

## KNXnetIP/Tunneling
Когда выполнено подключение, шлюз отслеживает его состояния и переподключается если связь разрывается.

Подключиться можно из скриптов:
```lua
require("knx")

knx.beginIP()
knx.connect("172.16.1.88", 3671)
```

Отключение от IP Interface:
```lua
require("knx")

knx.disconnect()
```

Возможно обнаружение в сети KNX IP Interface через функцию Discovery:
```lua
require("knx")

knx.beginIP()
knx.discovery()
```

Информация об обнаруженных интерфейсах отображается в логе.

## Типы данных
Стандарт KNX содержит несколько сотен типов данных, но используются в большинстве случаев их небольшое число. 

Если тип для группового объекта не задан (Undefined), то используется его фактическое представление (Raw), если его длина 1-6 бит, то он содержится в первом байте значения, если длина 1 байт и больше, что первый байт значения должен быть 0x00.

Ознакомится с полным списком возможных типов данных можно в официальной документации [KNX](https://www.knx.org/wAssets/docs/downloads/Certification/Interworking-Datapoint-types/03_07_02-Datapoint-Types-v02.02.01-AS.pdf).

### Поддерживаемые типы данных
* 1.001 DPT_Switch (Off / On)
* 3.007 DPT_Control_Dimming
* 5.000 DPT_1ByteUInt (Базовый тип, целое число 0 .. 255)
* 5.001 DPT_Scaling (Диапазон 0 .. 100 %)
* 5.003 DPT_Angle (Диапазон 0 .. 360 °)
* 6.000 DPT_1ByteInt (Базовый тип, целое число -128 .. 127)
* 7.000 DPT_2ByteUInt (Базовый тип, целое число 0 .. 65 535)
* 8.000 DPT_2ByteInt (Базовый тип, число -32 768 ... 32 767)
* 9.001 DPT_Value_Temp
* 10.001 DPT_TimeOfDay
* 11.001 DPT_Date
* 17.001 DPT_SceneNumber
* 19.001 DPT_DateTime

Новые типы данных будут добавляться по мере необходимости.

## Просмотр базы 
Просмотр базы групповых объектов производится в веб-интерфейсе на странице */knx*.

## MQTT
Для использования шлюза KNX-MQTT включите поддержку при инициализации *knx.beginIP(true)*.

Шлюз публикует значение сконфигурированного группового объекта в топик вида:

**knx/{GROUP_NAME}/{NAME}**



Для изменения значения группового необходимо передать значение в топик вида:

**knx/{GROUP_NAME}/{NAME}/set**



Для отправки запроса на чтение значения группового необходимо отправить любое значение в топик:

**knx/{GROUP_NAME}/{NAME}/get**

## HTTP
Прямое управление в разработке, на данный момент HTTP управление можно использовать через объекты.

## Скрипты
Конфигурация группового объекта, задание типа и названия, а так же расположения:
```lua
require("knx")

knx.groupConf("1/2/200", "DPT_Value_Temp", "Actual temperature", "room_1")
```

Запрос на чтение группового объекта:
```lua
require("knx")

knx.groupRead("1/1/10")
```
Полученные данные будут сохранены в БД.


Запрос на запись в групповой объект:
```lua
require("knx")

knx.groupWrite("1/1/10", 0x01)
```

Получение значения группового объекта из базы:
```lua
require("knx")

local sw = knx.groupValue("1/1/10")
print(sw)
```
