### Упрощенная отправка сообщений в телеграм (начиная с версии 20200915)

1. Зарегистрировать своего бота можно у [@BotFather](https://t.me/BotFather). Во время регистрации будет выдан *token*.
2. Узнать свой *ChatId* можно у бота [@userinfobot](https://t.me/userinfobot).

*token* и *ChatId* достаточно написать 1 раз в `init.lua`, потом использовать только `telegram.send()` :


```lua
-- добавьте в init.lua
telegram.settoken("5961....:AAHJP4...")
telegram.setchat("5748.....")

-- добавьте в любой lua скрипт где требуется оповещение
telegram.send("Температура: " .. string.format("%.2f", zigbee.value(tostring(Event.ieeeAddr), "temperature")) .. "°C, Влажность: " .. string.format("%.2f",zigbee.value(tostring(Event.ieeeAddr), "humidity")) .. "%")
```
### Отправка сообщения в телеграм с помощью вашего бота

```lua
local char_to_hex = function(c)
  return string.format("%%%02X", string.byte(c))
end

function round(exact, quantum)
    local quant,frac = math.modf(exact/quantum)
    return quantum * (quant + (frac > 0.5 and 1 or 0))
end

local function urlencode(url)
  if url == nil then
    return
  end
  url = url:gsub("\n", "\r\n")
  url = url:gsub("([^%w ])", char_to_hex)
  url = url:gsub(" ", "+")
  return url
end

local hex_to_char = function(x)
  return string.char(tonumber(x, 16))
end

function SendTelegram(text)
  local token = "5177...:AAG0b...."  --
  local chat_id = "38806....."
  --http.request("https://api.telegram.org/bot" .. token .. "/sendMessage?chat_id=" .. chat_id .. "&text=" .. tostring(text))  -- https пока не работает в lua
  http.request("http://212.237.16.93/bot" .. token .. "/sendMessage?chat_id=" .. chat_id .. "&text=" .. urlencode(text))
end  


local value = zigbee.value("0x00158D00036C1508", "temperature")
local text = "temperature: " .. round(tostring(value),2)
SendTelegram(text)
```
