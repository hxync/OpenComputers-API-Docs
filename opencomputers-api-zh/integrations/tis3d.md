# tis3d

## 概述

本页说明 OpenComputers 与 TIS-3D 串口适配器之间的桥接接口。
启用后，Lua 可以通过 `serial_port` 组件与 TIS-3D 串行网络交换有符号 16 位数值。

## 可用性

- 依赖：`tis3d`
- 标签：`integration-required`

## 组件名

- `serial_port`

## 队列模型

桥接内部维护两条彼此独立的 FIFO 队列：

- 读队列：保存由 TIS-3D 写入、等待 Lua 读取的值
- 写队列：保存由 Lua 写入、等待 TIS-3D 读取的值

这两条队列的容量都为 `128`。

## `serial_port`

`serial_port` 由 TIS-3D 串口适配器桥接环境暴露。

### `setReading`

- 语法：`serial_port.setReading(enabled)`
- 参数：
  - `enabled`：布尔值
- 返回：无
- 用途：开启或关闭来自 TIS-3D 一侧的入站接收

关闭读取后，TIS-3D 一侧将无法继续向 OpenComputers 的读队列压入新值。

示例：

```lua
component.serial_port.setReading(true)
```

### `read`

- 语法：`serial_port.read()`
- 返回：成功时返回一个数字；队列为空时不返回任何值
- 用途：取出并返回读队列中最早进入的一项数据

重要行为：

- Lua 侧看到的是普通有符号数字，底层来源于 Java `short`
- 队列为空不算错误，也不会返回错误字符串

示例：

```lua
local value = component.serial_port.read()
if value ~= nil then
  print("Received:", value)
end
```

### `write`

- 语法：`serial_port.write(value)`
- 参数：
  - `value`：要发送给 TIS-3D 的 16 位数值
- 返回：成功时返回 `true`；写队列已满时返回 `false, "buffer full"`
- 用途：向出站队列追加一个待发送值

如果写队列中已经积压了 `128` 个值，新值就会被拒绝。

示例：

```lua
local ok, reason = component.serial_port.write(42)
print("Write:", ok, reason)
```

## 使用说明

- Lua `write()` 写入的是给 TIS-3D 读取的那条队列。
- Lua `read()` 读取的是由 TIS-3D 写入给 OpenComputers 的那条队列。
- 当需要时，桥接会自动把自身接入适配器网络。
- 队列中的缓存值会随着适配器状态一起写入存档，因此区块保存后通常不会立刻丢失。

## 完整示例

```lua
local component = require("component")
local port = component.serial_port

port.setReading(true)

local ok, reason = port.write(42)
print("Write:", ok, reason)

local value = port.read()
if value ~= nil then
  print("Received:", value)
end
```

## 相关内容

- `modem`
- `redstone`
