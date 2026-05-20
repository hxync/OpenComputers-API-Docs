# chat_box

## 概述

方块版 `chat_box` 组件是 Computronics 提供的放置式聊天接口。它可以向附近玩家发送聊天消息，也可以监听附近玩家的聊天内容，在某些配置下还可以把“收到聊天”转成短暂的红石信号。

和机器人升级版相比，方块版在创造模式变体以及全局广播行为上更复杂一些。

## 可用性

- 仓库：`computronics`
- 组件名：`chat_box`
- 常见宿主：放置在世界中的 Computronics 聊天盒方块

## 用法

```lua
local component = require("component")
local chatBox = component.chat_box

if not chatBox then
  error("未安装 chat_box 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 聊天行为

- 默认配置距离通常为 `40` 格。
- 该方块会监听附近玩家聊天，并产生一个 `computer.signal` 事件：

```lua
"chat_message", username, message
```

- 如果配置中启用了红石刷新，收到聊天后方块会短时间输出红石信号。
- 创造版聊天盒行为不同：
  - 它可以保留更大甚至近似无限的距离值
  - 在没有显式设置距离时，它可以进行全局广播

## 接口

### say

- 语法：`chatBox.say(text[, distance])`
- 返回值：`boolean`
- 用途：让聊天盒发送一条聊天消息。

参数说明：

- `text:string`：消息正文。
- `distance:number` 可选：仅本次发送使用的临时半径覆盖值。

行为说明：

- 如果设置了自定义名称，就用它作为聊天前缀。
- 否则使用配置里的默认前缀，通常为 `ChatBox`。
- 如果省略 `distance`：
  - 普通聊天盒使用已保存的距离
  - 创造版或 magic-chat 行为下，可能允许全局发送
- 如果 `distance <= 0`，则继续使用已保存距离。
- 只要传入的是字符串消息，这个接口就会返回 `true`；否则返回 `false`。

示例：

```lua
chatBox.setName("BaseConsole")
assert(chatBox.say("Perimeter check complete."))
```

### getDistance

- 语法：`chatBox.getDistance()`
- 返回值：`number`
- 用途：返回当前保存的本地聊天半径。

示例：

```lua
print("当前保存距离:", chatBox.getDistance())
```

### setDistance

- 语法：`chatBox.setDistance(distance)`
- 返回值：`number`
- 用途：设置保存的聊天半径，并返回最终生效值。

参数说明：

- `distance:number`：希望设置的半径。

行为说明：

- 大于 `32767` 的值会先被压缩。
- 普通聊天盒会继续被限制到配置允许的最大聊天距离。
- 创造版聊天盒可以保留更大的值。
- 负数会把组件重置回配置默认距离，同时清除内部“显式设置过距离”的标记。

示例：

```lua
print("生效距离:", chatBox.setDistance(24))
```

### getName

- 语法：`chatBox.getName()`
- 返回值：`string`
- 用途：返回当前保存的聊天前缀名称。

示例：

```lua
print("当前前缀:", chatBox.getName())
```

### setName

- 语法：`chatBox.setName(name)`
- 返回值：`string`
- 用途：设置自定义聊天前缀名称，并返回新值。

参数说明：

- `name:string`：新的前缀名称。

示例：

```lua
chatBox.setName("Operations")
```

## 示例

对附近聊天内容作出回复：

```lua
local event = require("event")

while true do
  local _, _, _, signal, user, message = event.pull("computer.signal")
  if signal == "chat_message" and message == "ping" then
    chatBox.say("pong")
  end
end
```

## 说明

- 如果服务器启用了 magical chat 行为，这个聊天盒的监听和发送范围可能会超出普通本地半径的限制。
- 方块版会持久化保存名称以及“是否显式设置过距离”这两个状态。

## 相关内容

- `component`
- `chat`
