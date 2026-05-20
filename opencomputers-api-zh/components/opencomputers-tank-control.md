# opencomputers-tank-control

## 概述

本页记录内部储罐选择与转移相关的回调接口。这些 API 操作的是宿主设备自身内置的储罐。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：由具备储罐能力的宿主提供，例如 `robot`
- 底层实现特征：`TankControl`

## 储罐编号

Lua 侧储罐索引从 `1` 开始。

## API

### tankCount

- 语法：`device.tankCount()`
- 返回值：`number`
- 用途：返回宿主内部储罐的总数量。

### selectTank

- 语法：`device.selectTank([index])`
- 返回值：`number`
- 用途：读取或修改当前选中的内部储罐。

### tankLevel

- 语法：`device.tankLevel([index])`
- 返回值：`number`
- 用途：返回指定储罐或当前选中储罐中的流体数量。

空储罐返回 `0`。

### tankSpace

- 语法：`device.tankSpace([index])`
- 返回值：`number`
- 用途：返回指定储罐或当前选中储罐的剩余容量。

### compareFluidTo

- 语法：`device.compareFluidTo(index)`
- 返回值：`boolean`
- 用途：按流体类型比较当前选中储罐与另一个内部储罐。

行为：

- 两个空储罐比较结果为 `true`。
- 一个空一个非空时结果为 `false`。

### transferFluidTo

- 语法：`device.transferFluidTo(index[, count])`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：把当前选中储罐中的流体移动到另一个内部储罐。

可能失败原因：

- `incompatible or no fluid`
- `invalid index`

行为细节：

- `count` 默认是 `1000` 毫桶。
- 如果源和目标本来就是同一个储罐，会直接返回 `true`。
- 如果普通转移失败，但两个储罐都能容纳对方当前内容，实现会尝试直接交换两边流体。

## 示例

```lua
local robot = require("robot")

print("Internal tank count:", robot.tankCount())
print("Selected tank:", robot.selectTank())

robot.selectTank(1)
print("Selected tank level:", robot.tankLevel())
print("Selected tank free space:", robot.tankSpace())
print("Same fluid as tank 2:", robot.compareFluidTo(2))

local ok, reason = robot.transferFluidTo(2, 500)
print("Transfer result:", ok, reason)
```

## 相关内容

- `opencomputers-tank-inventory-control`
- `opencomputers-tank-world-control`
- `robot`
