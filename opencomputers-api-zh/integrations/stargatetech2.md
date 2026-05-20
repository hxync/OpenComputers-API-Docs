# stargatetech2

## 概述

本页说明 OpenComputers 向 Lua 暴露的 StargateTech 2 抽象总线集成接口。

## 可用性

- 依赖: `stargatetech2`
- 标签: `integration-required`

## 组件名

- `abstract_bus`

## 主要用途

- 启用或禁用本地抽象总线接口。
- 读取和修改本地总线地址。
- 扫描总线中的其他设备。
- 通过抽象总线发送小型结构化数据包。

## 组件

### abstract_bus

`abstract_bus` 由 StargateTech 2 抽象总线卡暴露。

#### `getEnabled()`

- 语法: `getEnabled(): boolean`
- 用途: 返回本地总线接口当前是否启用。
- 返回值:
  - `true` 表示启用。
  - `false` 表示禁用。

#### `setEnabled(enabled)`

- 语法: `setEnabled(enabled: boolean): boolean`
- 用途: 启用或禁用本地总线接口。
- 参数:
  - `enabled: boolean`
    目标启用状态。
- 返回值:
  - 新的启用状态。

#### `getAddress()`

- 语法: `getAddress(): number`
- 用途: 返回本地接口地址。
- 返回值:
  - 当前 16 位总线地址。

#### `setAddress(address)`

- 语法: `setAddress(address: number): number`
- 用途: 设置本地接口地址。
- 参数:
  - `address: number`
    新地址，只保留低 16 位。
- 返回值:
  - 实际保存的 16 位地址。

#### `send(address, data)`

- 语法: `send(address: number, data: table): table[, string]`
- 用途: 向抽象总线发送一个结构化数据包。
- 参数:
  - `address: number`
    目标总线地址。
  - `data: table`
    会被序列化成 LIP 数据包的键值表。
- 返回值:
  - 成功时返回响应表。
  - 如果本地节点能量不足，则返回 `nil, "not enough energy"`。
- 说明:
  - 发送前，所有键和值都会转换成字符串。
  - 总包长度受 OpenComputers `maxNetworkPacketSize` 配置限制。
  - 如果包过大，会触发类似 `packet too big` 的参数错误。

#### `scan(mask)`

- 语法: `scan(mask: number): table[, string]`
- 用途: 扫描抽象总线中可见的设备。
- 参数:
  - `mask: number`
    16 位扫描掩码。
- 返回值:
  - 成功时返回发现的设备表。
  - 能量不足时返回 `nil, "not enough energy"`。

#### `maxPacketSize()`

- 语法: `maxPacketSize(): number`
- 用途: 返回当前 OpenComputers 配置允许的最大包大小。
- 返回值:
  - 以字符数表示的最大包长度。

#### 事件: `bus_message`

- 语法: `bus_message(protocolId, sender, target, data, metadata)`
- 用途: 当拥有该组件的机器收到兼容抽象总线数据包时，向程序发送事件。
- 参数:
  - `protocolId: number`
    收到的数据包协议 id。
  - `sender: number`
    源地址。
  - `target: number`
    目标地址。
  - `data: table`
    收到的键值负载。
  - `metadata: table`
    元数据表，通常包含 `mod`、`device`、`player` 等字段。

## 示例

```lua
local component = require("component")
local bus = component.abstract_bus

print("Address:", bus.getAddress())
print("Enabled:", bus.getEnabled())

local devices = bus.scan(0xFFFF)
print("Found devices:", #devices)
```

## 相关内容

- `gc`
- `network`
