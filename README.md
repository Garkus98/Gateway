# Zigbee + BLE шлюз

Этот продукт  предназначен для работы с распространенными устройствами ZigBee, BLE.  В основе шлюза лежит контроллер [ESP32 от Espressif ](https://www.espressif.com/sites/default/files/documentation/esp32-wrover_datasheet_en.pdf). В качестве связущего звена протокола Zigbee  выступает тандем чипов от Texas Instruments [ZIgbee CC2538](https://www.ti.com/product/CC2538?utm_source=google&utm_medium=cpc&utm_campaign=epd-null-null-GPN_EN-cpc-pf-google-wwe&utm_content=CC2538&ds_k=%7b_dssearchterm%7d&DCM=yes&gclid=CjwKCAiA35rxBRAWEiwADqB37x__0Gm1rR2TUfCBETyuqrLjOtof6TuYSD3ZHzINYdNAbrXqfDxrwRoCpToQAvD_BwE&gclsrc=aw.ds) и  усилителя  [сс2592](https://www.ti.com/product/CC2592?utm_source=google&utm_medium=cpc&utm_campaign=epd-null-null-GPN_EN-cpc-pf-google-wwe&utm_content=CC2592&ds_k=%7b_dssearchterm%7d&DCM=yes&gclid=CjwKCAiA35rxBRAWEiwADqB3776CVlMD1GHdk-unOn9R0YeMtlwAnjUv-CIPuWvjhNqZRbiq6zy-ExoCxjYQAvD_BwE&gclsrc=aw.ds), либо готовый чип от [NXP JN5168](https://www.nxp.com/products/wireless/zigbee/zigbee-and-ieee802.15.4-wireless-microcontroller-with-256-kb-flash-32-kb-ram:JN5168). Для связи с устройствами по протоколу BLE используются встроенные возможности ESP32.


# Общие сведения
Шлюз выполняет роль координатора Zigbee и позволяет:

1) Использовать большинство доступного Zigbee оборудования. Список поддерживаемого и протестированного обрудования доступен по [ссылке](/devices/devices.md). Новое оборудование может быть добавлено после обсуждения с нами.

2) Отказаться от необходимости использования облаков производителей устройств. В качестве альтернативы, предлагается использовать облачный сервис [Smart Logic System](https://cloud.slsys.io), либо нативные приложения для Android и Apple iPhone (в разработке). 

3) Использовать распространенные  локальные системы автоматизации, такие как [MajorDomo](https://mjdm.ru/), [ioBroker Smarthome](https://www.iobroker.net), [HomeAssisiant](https://www.home-assistant.io), [Node-Red](https://nodered.org) и др. Для интеграции с этими системами используется протокол MQTT. Структура топиков протокола MQTT идентична  проекту  [zigbee2mqtt](https://www.zigbee2mqtt.io), поэтому для использования и интеграции шлюза нет необходимости изучать скриптовые языки указанных выше систем, так как протокол в основном уже доступен с помощью  модулей расширения.


# Дополнительные возможности шлюза через Web интерфейс
1. Управление и просмотр сведений  устройств через Web интерфейс шлюза по адресу http://ipadress (80 порт). Возможность отображения источника питания, уровня заряда батареи, доступных [EndPoint устройств](https://community.nxp.com/thread/332332)  в web-интерфейсе.

2. Создание локальных автоматизаций внутри шлюза [Simplelink](/simplelink.md).

3. Возможность написания сценариев на языке [Lua](https://ru.wikipedia.org/wiki/Lua) [Книга по Lua на русском языке](https://www.htbook.ru/kompjutery_i_seti/programmirovanie/programmirovanie-na-yazyke-lua).

4.	Возможность создания групп для управления несколькими устройствами одновременно (в разработке).

5.	Возможность задавать имя устройству. Если вы планируете использовать  шлюз с локальными системами автоматизации, рекомендуется установить галочку отправки адреса вместо устройств.

5.	Возможность удаления устройства. 

6.	Возможность отображения маршрутов в web-интерфейсе (в разработке).

8.	Возможность установить прямые связи [Bind](/bind.md) между устройствами ZigBee без участия координатора для управления конечными устройствами.

9.	Возможность управлять аппаратными [светодиодами (адресными или RGB)](/faq.md). 

10.	Возможность управлять звуком (при наличии распаянного усилителя) (в разработке)

11.	Возможность изменить PanId и номер канала.

12.	Возможность задать имя шлюза в сети.

13.	Возможность перехода шлюза в режим АР при нажатии аппаратной кнопки в течение 2-5 секунд после подачи питания.

14.	Список поддерживаемых устройств постоянно обновляется (информация находится в файле converters.txt в архиве с прошивкой)



# Аппаратная часть
Устройство можно [собрать самостоятельно](https://modkam.ru/?p=1342), или приобрести на сайте [Smart Logic System](slsys.io)



# Прошивка устройства
[Постоянная ссылка на прошивку устроуства](https://github.com/slsys/Gateway/raw/master/rom/zigbee_gw_full.7z)

[История изменений прошивки](/rom/history_ru.md)

Для прошивки запустить соответствующий батник из архива.
При первом запуске, создается точка доступа c именем вида zgwABCD, без пароля.
После подключения к ней, автоматически открывается страница настроек (если не открылась, можно зайти по адресу 192.168.4.1) и прописываем подключение к точке доступа и к MQTT серверу (но его можно указать и позже), нажимаем перезагрузку и шлюз подключится к точке доступа и начнет слать сообщения в MQTT. В случае возникновения проблем с доступом к captive portal, рекомендуется отключать GPRS на Android смартфонах. Обновление прошивок можно производить через Web интерфейс приложения.

Замечание: существует две версии прошивки, для чипов с 4мб и 16 мб FLASH RAM. Версии отличаются наличием возможности производить обновление через OTA.



# Полезные ссылки:


## [Первый запуск](/firststart.md)

## [Web-интерфейс](/web.md)


## [Simplelink](/simplelink.md)


## [Binding](/bind.md)


## [FAQ (часто задаваемые вопросы)](/faq.md)



# Интеграции

## [Интеграция с Majordomo](/int_majordomo.md)

## [Интеграция с HomeAssistant](/int_has.md)  (в разработке)

## [Интеграция с Node-Red](/int_nodered.md)   (в разработке)

## [Интеграция с IObroker](/int_iob.md)  (в разработке)

## [Интеграция с Алисой Яндекс](/int_yandex.md)  (в разработке)

## [Интеграция с Google Home](/int_google.md)  (в разработке)

## [Интеграция с HomeKit](/int_homekit.md)  (в разработке)
