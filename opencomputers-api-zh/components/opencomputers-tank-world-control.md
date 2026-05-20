# opencomputers-tank-world-control

## 概述

本页记录在宿主当前选中内部储罐与相邻世界储罐之间搬运流体的回调接口。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：具备储罐能力的宿主，例如 `robot`
- 底层实现特征：`TankWorldControl`

## API

### compareFluid

- 语法：`device.compareFluid(side[, tank])`
- 返回值：`boolean`
- 用途：比较当前选中内部储罐与指定相邻侧面的储罐是否是同一种流体。

行为：

- 如果省略 `tank`，会检查该侧面上任意一个储罐是否含有相同流体类型。
- 如果传入 `tank`，则只比较那个精确的相邻储罐索引。

### drain

- 语法：`device.drain(side[, amount])`
- 返回值：
  - 成功：`true, movedAmount`
  - 失败：`nil, reason`
- 用途：把相邻储罐中的流体抽入当前选中的内部储罐。

可能失败原因：

- `tank is full`
- `incompatible or no fluid`
- `no tank selected`

### fill

- 语法：`device.fill(side[, amount])`
- 返回值：
  - 成功：`true, movedAmount`
  - 失败：`nil, reason`
- 用途：把当前选中内部储罐中的流体推出到相邻储罐。

可能失败原因：

- `tank is empty`
- `incompatible or no fluid`
- `no space`
- `no tank selected`

## 使用说明

- `amount` 默认是 `1000` 毫桶。
- 当指定相邻储罐索引时，同样使用从 `1` 开始的编号。
- 传入 `0` 数量时，底层实现会把它当成一个合法但不产生实质搬运的调用路径。

## 示例

```lua
local robot = require("robot")
local sides = require("sides")

robot.selectTank(1)

print("Same fluid as left tank:", robot.compareFluid(sides.left, 1))

local ok, moved = robot.drain(sides.left, 1000)
print("Pulled from world tank:", ok, moved)

local ok2, moved2 = robot.fill(sides.right, 500)
print("Pushed into world tank:", ok2, moved2)
```

## 相关内容

- `opencomputers-tank-control`
- `opencomputers-world-tank-analytics`
- `robot`
