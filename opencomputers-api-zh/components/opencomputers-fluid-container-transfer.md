# opencomputers-fluid-container-transfer

## 概述

本页记录在储罐方块与库存中的流体容器之间搬运流体的回调接口。这些函数主要由 `transposer` 组件提供。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：`transposer`
- 底层实现特征：`FluidContainerTransfer`

## 通用规则

- 库存槽位从 `1` 开始编号。
- 流体数量单位为毫桶。
- 省略 `count` 时默认按一桶处理。
- 如果相邻库存不存在，会返回 `nil, "no inventory"`。

## API

### transferFluidFromTankToContainer

- 语法：`transposer.transferFluidFromTankToContainer(tankSide, inventorySide, inventorySlot[, count[, sourceTank[, outputSide[, outputSlot]]]])`
- 返回值：
  - 成功：`boolean, number`
  - 失败：`nil, reason`
- 用途：把指定侧面储罐或流体处理器中的流体装入某个容器物品。

返回值：

- 第一个值表示是否真的移动了流体。
- 第二个值表示实际移动了多少毫桶。

参数：

- `tankSide:number`：源储罐所在侧面。
- `inventorySide:number`：存放容器物品的库存所在侧面。
- `inventorySlot:number`：源容器物品所在槽位。
- `count:number`：最多移动多少流体。
- `sourceTank:number`：可选，源储罐索引，从 `1` 开始。
- `outputSide:number`：可选，处理后容器物品应放入的输出库存侧面。
- `outputSlot:number`：可选，精确输出槽位。

### transferFluidFromContainerToTank

- 语法：`transposer.transferFluidFromContainerToTank(inventorySide, inventorySlot, tankSide[, count[, outputSide[, outputSlot]]])`
- 返回值：
  - 成功：`boolean, number`
  - 失败：`nil, reason`
- 用途：把容器物品中的流体倒入相邻储罐或流体处理器。

### transferFluidBetweenContainers

- 语法：`transposer.transferFluidBetweenContainers(sourceSide, sourceSlot, sinkSide, sinkSlot[, count[, sourceOutputSide[, sinkOutputSide[, sourceOutputSlot[, sinkOutputSlot]]]]])`
- 返回值：
  - 成功：`boolean, number`
  - 失败：`nil, reason`
- 用途：把一个容器物品中的流体转移到另一个容器物品中。

这个操作只有在两边处理后的容器物品都能被正确放回指定输出位置时，才会真正提交。

## 使用说明

- 如果没有指定输出侧面或输出槽位，处理后的容器通常会回到原来源库存。
- 某些双容器操作会先模拟输出位置是否可放置，确认成功后才真正提交，以避免出现只完成一半的转换。
- 宿主自己的传输守卫还可能返回额外失败原因。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

local ok, amount = transposer.transferFluidFromTankToContainer(
  sides.left,   -- tank
  sides.right,  -- inventory
  1,            -- container slot
  1000,
  1
)
print("Filled container:", ok, amount)

local emptied, moved = transposer.transferFluidFromContainerToTank(
  sides.right,
  2,
  sides.left,
  500
)
print("Drained container:", emptied, moved)
```

## 相关内容

- `opencomputers-inventory-transfer`
- `opencomputers-world-fluid-container-analytics`
- `opencomputers-world-tank-analytics`
