# opencomputers-wake-message-aware

## 概述

本页记录具备“唤醒消息”能力的网络类组件共享的回调接口。这类组件可以在收到匹配网络包时启动已经停止的计算机。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：由支持唤醒的网络组件提供，例如某些 modem
- 底层实现特征：`WakeMessageAware`

## 作用

- 保存一个可选的唤醒消息字符串。
- 可选开启模糊匹配，使额外的包参数被忽略。
- 当收到匹配的网络包时，启动宿主计算机。

## 匹配规则

- 精确模式：数据包里必须恰好只有一个消息项，并且该消息项与配置的唤醒消息完全一致。
- 模糊模式：只要求第一个消息项匹配，后续附加参数会被忽略。
- 匹配时既接受 Lua 字符串，也接受能解码成相同 UTF-8 文本的原始字节数组。

唤醒匹配不会检查端口号，因为计算机停止时端口本来就是关闭的。

## API

### getWakeMessage

- 语法：`component.getWakeMessage()`
- 返回值：`string 或 nil, boolean`
- 用途：返回当前配置的唤醒消息以及模糊匹配标志。

### setWakeMessage

- 语法：`component.setWakeMessage(message[, fuzzy])`
- 返回值：`旧消息或 nil, 旧 fuzzy 标志`
- 用途：修改唤醒消息配置，并返回旧配置。

行为：

- 如果第一个参数传入 `nil`，会完全清除当前唤醒消息。
- 如果省略 `fuzzy`，则保留之前的模糊匹配设置。

## 示例

```lua
local component = require("component")
local modem = component.modem

local oldMessage, oldFuzzy = modem.setWakeMessage("wake", true)
print("Previous wake config:", oldMessage, oldFuzzy)

local message, fuzzy = modem.getWakeMessage()
print("Current wake config:", message, fuzzy)
```

## 相关内容

- `modem`
- `opencomputers-redstone-signaller`
