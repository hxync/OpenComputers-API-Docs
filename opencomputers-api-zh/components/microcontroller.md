# microcontroller

## 概述

`microcontroller` 组件是已放置微控制器方块暴露出的控制接口。微控制器是一种更紧凑的计算设备，硬件容量有限，不能像完整计算机那样直接访问任意外部组件，并且只提供少量与电源状态和侧面消息转发有关的回调。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`microcontroller`
- 常见宿主：已放置的微控制器方块

## 作用

- 启动和停止内部机器。
- 查询内部机器是否正在运行，以及最近一次崩溃的原因。
- 控制哪些物理侧面允许在微控制器内部网络与外部网络之间转发 `network.message` 消息。

## 硬件说明

- 微控制器由微控制器外壳和内部部件组成，常见部件包括 CPU、内存、扩展卡和 EEPROM。
- 它比完整计算机更小、更便宜，但不能像普通机箱那样直接访问任意外部组件。
- 侧面相关 API 只适用于除方块正面朝向之外的另外五个面；把正面朝向作为参数传入会报错。
- 所有侧面的消息输出默认都是开启的。

## API

### start

- 语法：`microcontroller.start()`
- 返回值：`boolean`
- 用途：启动内部机器。

返回值：

- `true` 表示状态发生变化，机器已实际启动。
- `false` 表示它本来就在运行，或当前状态下无法启动。

### stop

- 语法：`microcontroller.stop()`
- 返回值：`boolean`
- 用途：停止内部机器。

### isRunning

- 语法：`microcontroller.isRunning()`
- 返回值：`boolean`
- 用途：检查内部机器当前是否正在运行。

### lastError

- 语法：`microcontroller.lastError()`
- 返回值：`string` 或 `nil`
- 用途：返回最近一次崩溃的原因；如果没有崩溃记录则返回 `nil`。

### isSideOpen

- 语法：`microcontroller.isSideOpen(side)`
- 参数：
  - `side:number`：OpenComputers 的侧面常量，且不能是方块正面。
- 返回值：`boolean`
- 用途：检查该侧面当前是否允许把消息转发出去。

### setSideOpen

- 语法：`microcontroller.setSideOpen(side, open)`
- 参数：
  - `side:number`：OpenComputers 的侧面常量，且不能是方块正面。
  - `open:boolean`：`true` 表示允许转发，`false` 表示阻止转发。
- 返回值：`boolean`
- 用途：修改该侧面的转发开关，并返回修改前的旧值。

## 网络行为

- 从外部网络收到的消息会被复制到微控制器内部网络。
- 由内部网络发出的消息会从所有处于开启状态的外部侧面转发出去。
- 关闭某个侧面只会阻止该侧面的消息转发，不会关闭整台机器。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local mcu = component.microcontroller

if not mcu.isRunning() then
  assert(mcu.start(), "microcontroller did not start")
end

local previous = mcu.setSideOpen(sides.north, false)
print("North side was previously open:", previous)
print("North side open now:", mcu.isSideOpen(sides.north))

local crash = mcu.lastError()
if crash then
  print("Last crash:", crash)
end
```

## 相关内容

- `computer`
- `robot`
- `eeprom`
- `redstone`
