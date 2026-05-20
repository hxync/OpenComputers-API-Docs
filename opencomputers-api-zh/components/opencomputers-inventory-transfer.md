# opencomputers-inventory-transfer

## 概述

本页记录转运器以及类似外部库存路由设备暴露出的转移接口。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：`transposer`
- 底层实现特征：`InventoryTransfer`

## 侧面规则

这些回调操作的是相邻方块，而不是设备自身库存。

- 侧面编号会受宿主朝向规则影响。
- 对于必须依赖相邻库存的物品操作，如果相邻容器不存在或不可用，会返回 `nil, "no inventory"`。

## API

### transferItem

- 语法：
  - `transposer.transferItem(sourceSide, sinkSide[, count[, sourceSlot[, sinkSlot]]])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, reason`
- 用途：把物品从一个相邻库存移动到另一个相邻库存。

返回值含义：

- 返回实际移动的物品数量。

可能失败原因：

- `no inventory`
- 组件内部守卫返回的宿主特定传输锁定原因

槽位参数从 `1` 开始编号。

### swap

- 语法：`transposer.swap(sourceSide, sinkSide, sourceSlot, sinkSlot[, safe])`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, reason`
- 用途：交换两个相邻库存中的指定槽位。

`safe` 默认为 `false`。当它为 `true` 时，实现要求两个槽位都非空，才会尝试交换。

### transferFluid

- 语法：`transposer.transferFluid(sourceSide, sinkSide[, count[, sourceTank]])`
- 返回值：
  - 成功：`boolean, number`
  - 失败：`nil, reason`
- 用途：把流体从一个相邻储罐或流体处理器移动到另一个。

返回值：

- 第一个值表示是否真的移动了流体。
- 第二个值表示实际移动了多少毫桶。

可能失败原因：

- 宿主守卫路径上的 `no inventory`
- `device has fluid transfer rate of 0`
- 宿主特定的传输锁定原因

`count` 默认是一桶，`sourceTank` 在传入时也按 `1` 开始编号。

### getFluidTransferRate

- 语法：`transposer.getFluidTransferRate()`
- 返回值：`number`
- 用途：返回设备配置的流体传输速率，单位为升每秒。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

local moved, reason = transposer.transferItem(sides.north, sides.south, 16, 1, 1)
if moved then
  print("Items moved:", moved)
else
  print("Item transfer failed:", reason)
end

local ok, amount = transposer.transferFluid(sides.west, sides.east, 1000, 1)
print("Fluid moved:", ok, amount)
print("Fluid rate:", transposer.getFluidTransferRate())
```

## 相关内容

- `opencomputers-fluid-container-transfer`
- `opencomputers-world-inventory-analytics`
- `opencomputers-world-tank-analytics`
