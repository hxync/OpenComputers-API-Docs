# chat

## 概述

机器人上的 `chat` 组件是 Computronics 提供的聊天接口，可以主动向游戏聊天栏发言，也可以监听附近玩家发送的聊天消息。

它很适合做通知机器人、NPC 式交互、运维终端以及对附近聊天内容作出反应的脚本。

## 可用性

- 仓库：`computronics`
- 组件名：`chat`
- 常见宿主：安装了 Computronics 聊天盒升级的机器人

## 用法

```lua
local component = require("component")
local chat = component.chat

if not chat then
  error("未安装 chat 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 聊天行为

- 默认聊天范围来自 Computronics 配置，默认值通常是 `40` 格。
- 自定义名称是可选的；如果未设置自定义名称，消息前缀默认使用 `ChatBox`。
- 收到附近玩家聊天时，会产生一个名为 `chat_message` 的 `computer.signal` 事件。

事件参数：

```lua
"chat_message", username, message
```

除非服务器配置启用了全局 magic-chat 行为，否则机器人只会接收到范围内玩家的聊天消息。

## 接口

### say

- 语法：`chat.say(text[, distance])`
- 返回值：`boolean`
- 用途：让机器人使用当前名称和范围向聊天栏发送一条消息。

参数说明：

- `text:string`：要发送的消息正文。
- `distance:number` 可选：仅对本次发送生效的临时范围覆盖值。

行为说明：

- 如果设置了自定义名称，聊天前缀就会使用这个名称。
- 如果没有设置，则退回到配置中的默认前缀，通常为 `ChatBox`。
- 如果提供了 `distance`，它会被限制到配置允许的最大聊天距离。
- 如果 `distance <= 0`，则继续使用当前已经保存的距离。
- 当传入的是合法字符串消息时，接口会返回 `true`。
- 参数类型不符合预期时会返回 `false`。

机器人版本从 Lua 侧不会触发“无距离全局广播”模式。即便整合包启用了 magic chat，这里发送时仍然会带一个具体距离。

示例：

```lua
assert(chat.say("Systems online."))
assert(chat.say("Area clear within ten blocks.", 10))
```

### getDistance

- 语法：`chat.getDistance()`
- 返回值：`number`
- 用途：返回当前保存的发言与监听半径。

示例：

```lua
print("聊天半径:", chat.getDistance())
```

### setDistance

- 语法：`chat.setDistance(distance)`
- 返回值：`number`
- 用途：设置保存的聊天半径，并返回最终生效的新值。

参数说明：

- `distance:number`：请求设置的半径。

行为说明：

- 大于 `32767` 的值会先被截断。
- 之后还会继续限制到配置允许的最大聊天距离。
- 负数会把距离重置回配置默认值。

示例：

```lua
local effective = chat.setDistance(16)
print("当前聊天距离:", effective)
```

### getName

- 语法：`chat.getName()`
- 返回值：`string`
- 用途：返回当前设置的自定义发言名称。

空字符串表示尚未设置名称，此时会使用配置默认前缀。

示例：

```lua
print("当前聊天名称:", chat.getName())
```

### setName

- 语法：`chat.setName(name)`
- 返回值：`string`
- 用途：设置自定义发言名称，并返回保存后的值。

参数说明：

- `name:string`：显示在聊天前缀中的名称。

示例：

```lua
chat.setName("MaintenanceBot")
chat.say("Inspection complete.")
```

## 示例

监听附近玩家聊天并自动回复：

```lua
local event = require("event")

chat.setName("HelpBot")

while true do
  local _, _, _, signal, user, message = event.pull("computer.signal")
  if signal == "chat_message" and message == "status" then
    chat.say("All monitored systems are nominal.")
  end
end
```

## 相关内容

- `component`
- `speech`
