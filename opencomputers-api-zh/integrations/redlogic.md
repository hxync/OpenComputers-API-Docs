# redlogic

## 概述

本页说明 OpenComputers 对 RedLogic 灯具方块的集成接口。

## 可用性

- 依赖: `redlogic`
- 标签: `integration-required`

## 组件名

- `lamp`

## 主要用途

- 在自动化程序中读取灯的颜色。
- 检测灯当前是否通电。
- 区分不同类型的 RedLogic 灯具。

## 组件

### lamp

`lamp` 组件对应实现了 `ILampBlock` 的 RedLogic 灯方块。

#### `getLampColor()`

- 语法: `getLampColor(): number`
- 用途: 返回灯当前的 RGB 整数颜色值。
- 返回值:
  - 打包后的 RGB 整数颜色。
- 说明:
  - 显示时通常可以自行转换成十六进制格式。

#### `isLampPowered()`

- 语法: `isLampPowered(): boolean`
- 用途: 返回灯当前是否处于通电状态。
- 返回值:
  - `true` 表示当前已通电。
  - `false` 表示当前未通电。

#### `getLampType()`

- 语法: `getLampType(): string`
- 用途: 返回 RedLogic 内部定义的灯类型名称。
- 返回值:
  - 灯类型对应的枚举名称字符串。
- 说明:
  - 这些接口只能读取状态，不能直接控制灯的开关。

#### 示例

```lua
local component = require("component")
local lamp = component.lamp

print("Type:", lamp.getLampType())
print(string.format("Color: 0x%06X", lamp.getLampColor()))
print("Powered:", lamp.isLampPowered())
```

## 相关内容

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-bundled`
