# wireless modem

## 概述

本页说明的是 OpenComputers 无线网卡提供的无线 `modem` 组件。它在普通 modem API 的基础上增加了可配置的信号强度，使数据包可以按无线方式跨距离传输，视无线网络条件还可能跨维度通信。

旧生成页里常把它写成 `opencomputers-wireless-network-card`，但 Lua 里实际能访问到的组件名依然是 `modem`。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`modem`
- 常见宿主：无线网络卡

## 等级差异

- 一阶无线卡只能发送和接收无线数据包；
- 二阶无线卡既支持无线流量，也会参与有线流量。

这会体现在：

- 两个等级都会返回 `isWireless() == true`；
- 一阶返回 `isWired() == false`；
- 二阶返回 `isWired() == true`。

## 无线强度

强度值就是无线发送时的最大传播距离，单位按方块计算。强度越高意味着：

- 理论可达距离越远；
- 每次发送无线包消耗的能量越多。

源码会把强度限制在 `0 .. 当前等级允许的最大无线范围`。

## 用法

```lua
local component = require("component")
local modem = component.modem

if not modem or not modem.isWireless() then
  error("当前环境没有无线 modem")
end
```

## 接口

### getStrength

- 语法：`modem.getStrength()`
- 返回值：`number`
- 用途：读取当前无线发送范围。

### setStrength

- 语法：`modem.setStrength(strength)`
- 返回值：`number`
- 用途：修改无线发送范围，并返回最终生效的值。

参数说明：

- `strength:number`：期望设置的传输范围。

行为说明：

- 小于 `0` 的值会变成 `0`；
- 超过当前等级上限的值会被截断到最大允许值。

示例：

```lua
local applied = modem.setStrength(32)
print("无线范围:", applied)
```

## 继承的 Modem 接口

无线卡同样支持标准 modem API：

- `open`
- `close`
- `isOpen`
- `send`
- `broadcast`
- `maxPacketSize`
- `isWireless`
- `isWired`

端口规则和 `modem_message` 事件结构，请参考有线 `modem` 页面。

## 错误行为

当无线发送时强度大于零，网卡会按当前配置范围消耗能量。如果能量不足，`send` 和 `broadcast` 可能会失败，并抛出：

- `"not enough energy"`

## 示例

```lua
modem.open(42)
modem.setStrength(16)
modem.broadcast(42, "ping")
```

## 相关内容

- `modem`
- `tunnel`
- `access_point`
- `component`
