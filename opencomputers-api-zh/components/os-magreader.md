# os_magreader

## 概述

`os_magreader` 组件是 OpenSecurity 的磁条卡读卡器。它的 Lua 接口本身不多，但当已写入数据的磁条卡被刷过读卡器时，方块会发送一个可配置名称的信号事件。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_magreader`
- 常见宿主：放置在世界中的 OpenSecurity 磁条卡读卡器

## 用法

```lua
local component = require("component")
local mag = component.os_magreader

if not mag then
  error("未安装 os_magreader 组件")
end
```

## 刷卡行为

当有效磁条卡被刷过读卡器且设备能量足够时，它会发送：

```lua
computer.signal(eventName, user, data, uuid, locked, side)
```

默认事件名：

- `magData`

载荷字段说明：

- `user`：
  - 当启用 `magCardDisplayName` 配置时，为玩家显示名
  - 当该配置关闭时，固定为字面量 `"player"`
- `data`：卡中保存的数据字符串
- `uuid`：
  - 默认返回卡片真实 UUID
  - 若启用 `ignoreUUIDs`，则返回 `"-1"`
- `locked:boolean`：该卡是否已锁定禁止重写
- `side:number`：玩家刷卡时命中的方块面编号

补充行为：

- 每次真正发送事件前会消耗 `5` 点缓冲能量。
- 当读卡器处于空闲状态且玩家使用磁条卡时，会播放 `opensecurity:card_swipe` 音效。
- 如果卡片没有 `data` 标签，则不会发送任何信号。

## 接口

### greet

- 语法：`mag.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置问候语。

### setEventName

- 语法：`mag.setEventName(name)`
- 返回：`boolean`
- 用途：修改之后刷卡时使用的信号事件名。

参数说明：

- `name:string`：新的事件名。

行为说明：

- 该名称会保存到 NBT 中。
- 世界重新加载后，如果值为空或不存在，则会回退为 `magData`。

示例：

```lua
assert(mag.setEventName("doorCard"))
```

## 示例

```lua
local event = require("event")

mag.setEventName("doorCard")

while true do
  local _, _, _, eventName, user, data, uuid, locked, side = event.pull("computer.signal")
  if eventName == "doorCard" then
    print(user, data, uuid, locked, side)
  end
end
```

## 相关内容

- `component`
- `os_cardwriter`
