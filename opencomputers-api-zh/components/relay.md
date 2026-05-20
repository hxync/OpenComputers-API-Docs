# relay

## 概述

`relay` 组件是中继器方块的可编程接口。它负责桥接不同子网络，同时保持普通组件仍然局部可见；此外还可以通过无线网卡或链接卡扩展成长距离数据包转发器。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`relay`
- 常见宿主：中继器方块

## 作用

- 在有线子网络之间转发数据包，同时避免把所有组件直接暴露给整个大网络。
- 安装无线网卡后，可以额外进行无线转发。
- 安装链接卡后，可以通过该卡保存的 tunnel 标识进行跨链接隧道转发。
- 支持 repeater 模式，把接收到的无线包再次广播出去。

## 依赖硬件的行为

- 没有无线网卡时，无线范围实际上不可用。
- 安装一级或二级无线网卡后，中继器会获得一个可配置的无线转发范围。
- 安装链接卡后，中继器能够通过该卡保存的 tunnel id 把数据转发到对应的 linked-card 隧道中。

它还可以插入 CPU、内存和存储部件，用来提升转发延迟、吞吐量和内部队列容量。

## API

### getStrength

- 语法：`relay.getStrength()`
- 返回值：`number`
- 用途：返回当前配置的无线转发范围。

### setStrength

- 语法：`relay.setStrength(strength)`
- 参数：
  - `strength:number`：期望设置的无线转发范围。
- 返回值：`number`
- 用途：设置无线转发范围，并返回最终实际应用的值。

行为：

- 该值会被限制在当前已安装无线网卡所允许的 `0..maxWirelessRange` 范围内。
- 如果根本没有安装无线网卡，那么有效最大值就是 `0`。

### isRepeater

- 语法：`relay.isRepeater()`
- 返回值：`boolean`
- 用途：返回当前是否允许把接收到的无线包再次通过无线方式广播出去。

### setRepeater

- 语法：`relay.setRepeater(enabled)`
- 参数：
  - `enabled:boolean`：目标 repeater 状态。
- 返回值：`boolean`
- 用途：启用或禁用 repeater 模式，并返回修改后的状态。

## 使用说明

- 如果网络中存在中继环路，仍然可能出现重复包，因为该方块不会记录最近已经转发过哪些包。
- 只有当无线范围大于 `0` 且能量足够时，才会发生无线转发。
- linked-card 隧道转发只会作用于“从具体某个侧面进入”的数据包，不会对所有内部转发路径都生效。
- 实现里还带有 ComputerCraft 兼容逻辑，会为连接的 ComputerCraft 计算机镜像类似 modem 的事件。

## 示例

```lua
local component = require("component")
local relay = component.relay

print("Current strength:", relay.getStrength())
print("Repeater mode:", relay.isRepeater())

relay.setStrength(16)
relay.setRepeater(true)

print("Applied strength:", relay.getStrength())
print("Repeater mode now:", relay.isRepeater())
```

## 相关内容

- `access_point`
- `modem`
- `tunnel`
