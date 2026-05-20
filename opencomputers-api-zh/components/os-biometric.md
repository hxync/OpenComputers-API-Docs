# os_biometric

## 概述

`os_biometric` 是 OpenSecurity 提供的生物识别读取器组件。玩家使用该方块时，设备会读取该玩家的信息，并发送一个包含 UUID 或 UUID 的 Base64 编码字符串的信号事件，具体由 OpenSecurity 配置项 `returnRealUUID` 决定。

它适合做门禁控制、身份日志记录、安全门和身份认证流程。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_biometric`
- 常见宿主：放置在世界中的 OpenSecurity 生物识别器

## 用法

```lua
local component = require("component")
local bio = component.os_biometric

if not bio then
  error("未安装 os_biometric 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 事件行为

当某个玩家被该方块扫描时，组件会发送：

```lua
computer.signal(eventName, payload)
```

默认事件名：

- `bioReader`

载荷行为：

- 如果启用了 `OpenSecurity.returnRealUUID`，载荷就是玩家真实 UUID 字符串
- 否则载荷是该 UUID 字符串做 Base64 编码后的结果

## 接口

### greet

- 语法：`bio.greet()`
- 返回值：`string`
- 用途：返回设备内置的一句问候文本。

在这个版本里，返回文本固定为：

```text
Lasciate ogne speranza, voi ch'intrate
```

示例：

```lua
print(bio.greet())
```

### setEventName

- 语法：`bio.setEventName(name)`
- 返回值：`boolean`
- 用途：设置扫描玩家时发送的信号事件名。

参数说明：

- `name:string`：新的事件名。

行为说明：

- 该名称会保存到 NBT。
- 空名称不会在重载后保留；如果读取时没有有效存储名称，设备会退回到 `bioReader`。

示例：

```lua
assert(bio.setEventName("doorAuth"))
```

## 示例

```lua
local event = require("event")

bio.setEventName("doorAuth")

while true do
  local _, _, _, eventName, payload = event.pull("computer.signal")
  if eventName == "doorAuth" then
    print("扫描到的身份:", payload)
  end
end
```

## 相关内容

- `component`
- `os_cardwriter`
