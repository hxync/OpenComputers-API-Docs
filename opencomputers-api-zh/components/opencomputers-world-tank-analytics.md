# opencomputers-world-tank-analytics

## 概述

本页记录对相邻侧面上的储罐与流体处理器进行只读检查的回调接口。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：通常由 `transposer` 和其他具备储罐感知能力的设备提供
- 底层实现特征：`WorldTankAnalytics`

## 通用规则

- 侧面编号用于指定相邻方块。
- 可选的储罐索引从 `1` 开始。
- 如果要读取流体描述，要求在配置中启用物品堆检查。

## API

### getTankLevel

- 语法：`device.getTankLevel(side[, tank])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "no tank"`
- 用途：返回某个相邻储罐中的流体量。

如果省略 `tank`，实现会把该侧面上所有储罐中的流体量求和后返回。

### getTankCapacity

- 语法：`device.getTankCapacity(side[, tank])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "no tank"`
- 用途：返回某个相邻储罐的容量。

如果省略 `tank`，实现会返回该侧面所有储罐中的最大容量值。

### getFluidInTank

- 语法：`device.getFluidInTank(side[, tank])`
- 返回值：
  - 成功：单个储罐信息表，或全部储罐信息表数组
  - 失败：`nil, reason`
- 用途：返回某个相邻储罐，或该侧面全部储罐的流体信息。

可能失败原因：

- `no tank`
- `not enabled in config`

### getTankCount

- 语法：`device.getTankCount(side)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "no tank"`
- 用途：返回指定侧面暴露出的储罐数量。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

print("Tank count on top:", transposer.getTankCount(sides.top))
print("Total fluid on top:", transposer.getTankLevel(sides.top))
print("Top tank 1 capacity:", transposer.getTankCapacity(sides.top, 1))

local info = transposer.getFluidInTank(sides.top, 1)
if info and info.fluid then
  print("Fluid amount:", info.fluid.amount)
end
```

## 相关内容

- `opencomputers-tank-world-control`
- `opencomputers-fluid-container-transfer`
- `transposer`
