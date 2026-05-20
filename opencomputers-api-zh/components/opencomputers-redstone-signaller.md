# opencomputers-redstone-signaller

## 概述

本页记录 `redstone` 组件共享的唤醒控制回调。它们本身不负责直接读写红石，而是用于配置“当输入红石变化时，何时唤醒组件网络上的休眠计算机”。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`redstone`
- 底层实现特征：`RedstoneSignaller`

## 作用

- 保存一个数字型唤醒阈值。
- 在监控到红石状态变化时发出 `redstone_changed` 计算机信号。
- 当红石输入从低于阈值上升到大于等于阈值时，启动休眠中的计算机。

默认情况下，唤醒数据包只会发送给同一组件网络中的相邻计算机。

## API

### getWakeThreshold

- 语法：`redstone.getWakeThreshold()`
- 返回值：`number`
- 用途：读取当前用于红石触发启动的唤醒阈值。

### setWakeThreshold

- 语法：`redstone.setWakeThreshold(threshold)`
- 参数：
  - `threshold:number`：当输入向上跨过该值时，允许触发唤醒的最小新输入值。
- 返回值：`number`
- 用途：设置新的唤醒阈值，并返回旧阈值。

## 唤醒逻辑

只有同时满足下面两个条件时，才会触发唤醒：

- 旧输入值小于阈值；
- 新输入值大于等于阈值。

因此，持续保持高电平不会反复唤醒计算机；它必须先掉到阈值以下，再重新升上来。

## 相关信号

具备红石感知能力的组件会发出：

- 普通模拟红石变化：`"redstone_changed", side, oldValue, newValue`
- 捆绑红石变化：`"redstone_changed", side, oldValue, newValue, color`
- 无线红石变化：`"redstone_changed", "wireless", oldValue, newValue`

## 示例

```lua
local component = require("component")
local redstone = component.redstone

local old = redstone.setWakeThreshold(10)
print("Old threshold:", old)
print("Current threshold:", redstone.getWakeThreshold())
```

## 相关内容

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-bundled`
- `opencomputers-redstone-wireless`
