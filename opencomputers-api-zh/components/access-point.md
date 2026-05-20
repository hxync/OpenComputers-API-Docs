# access_point

## 概述

`access_point` 组件是已废弃的接入点方块暴露出的可编程接口。它的行为类似一个带无线能力的交换设备，可以在有线子网和无线网络之间中继数据包。

它还可以充当无线中继器，把接收到的无线包再次转发回无线介质中。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`access_point`
- 常见宿主：接入点方块

## 重要说明

- 上游已经把 access point 方块标记为废弃，更推荐使用 relay。
- 无线中继范围由 strength 控制。
- Repeater 模式决定收到的无线包是否还要再次通过无线方式转发出去。
- 和 switch 一样，如果网络拓扑形成环路，access point 可能造成重复包。

## 用法

```lua
local component = require("component")
local ap = component.access_point

if not ap then
  error("当前环境没有 access_point 组件")
end
```

## 接口

### getStrength

- 语法：`ap.getStrength()`
- 返回值：`number`
- 用途：读取 access point 在无线转发时使用的中继范围。

### setStrength

- 语法：`ap.setStrength(strength)`
- 返回值：`number`
- 用途：修改无线中继范围，并返回最终应用的值。

参数说明：

- `strength:number`：希望设置的中继范围。

这个版本源码中的限制逻辑写得比较异常，但从设计意图上看，应该仍把它当成不超过二阶无线最大范围的数值来使用。

### isRepeater

- 语法：`ap.isRepeater()`
- 返回值：`boolean`
- 用途：判断当前 access point 是否会把收到的无线包再次转发回无线网络。

当它关闭时，access point 仍可能继续执行其他方向上的有线/无线桥接；这个标志主要控制的是“无线再广播”行为。

### setRepeater

- 语法：`ap.setRepeater(enabled)`
- 返回值：`boolean`
- 用途：开启或关闭 repeater 模式，并返回新状态。

参数说明：

- `enabled:boolean`：目标 repeater 状态。

## 中继行为

运行时，access point 会：

- 像交换设备一样在已连接侧之间中继数据包；
- 在满足条件时额外把数据包转发到无线网络；
- 按当前强度为无线重发消耗能量。

它不会完整记录最近转发过的所有包，因此如果网络中存在环路，就可能收到重复投递的数据。

## 示例

```lua
print("当前是否中继:", ap.isRepeater())
ap.setRepeater(true)
ap.setStrength(24)
print("当前范围:", ap.getStrength())
```

## 相关内容

- `modem`
- `wireless modem`
- `relay`
- `component`
