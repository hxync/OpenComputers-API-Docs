# opencomputers-tank-inventory-control

## 概述

本页记录在宿主内部储罐与宿主库存中的流体容器物品之间搬运流体的回调接口。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：具备储罐能力的宿主，例如 `robot`
- 底层实现特征：`TankInventoryControl`

## 编号规则

- 库存槽位从 `1` 开始。
- 内部储罐索引从 `1` 开始。
- 流体数量单位为毫桶。

## API

### getTankLevelInSlot

- 语法：`device.getTankLevelInSlot([slot])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "item is not a fluid container"`
- 用途：返回当前选中槽位或指定槽位中容器物品的当前流体量。

### getTankCapacityInSlot

- 语法：`device.getTankCapacityInSlot([slot])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "item is not a fluid container"`
- 用途：返回当前选中槽位或指定槽位中容器物品的总容量。

### getFluidInTankInSlot

- 语法：`device.getFluidInTankInSlot([slot])`
- 返回值：
  - 成功：流体描述表
  - 失败：`nil, "item is not a fluid container"` 或 `nil, "not enabled in config"`
- 用途：返回当前选中槽位或指定槽位中容器物品的流体描述。

### getFluidInInternalTank

- 语法：`device.getFluidInInternalTank([tank])`
- 返回值：
  - 成功：流体描述表或 `nil`
  - 失败：`nil, "not enabled in config"`
- 用途：返回某个内部储罐中的流体描述。

### drain

- 语法：`device.drain([amount])`
- 返回值：
  - 成功：`true, movedAmount`
  - 失败：`nil, reason`
- 用途：把当前选中库存槽位中的流体转移到当前选中内部储罐。

可能失败原因包括：

- `nothing selected`
- `item is empty or not a fluid container`
- `tank is full`
- `incompatible fluid`
- `incompatible or no fluid`
- `no tank`

### fill

- 语法：`device.fill([amount])`
- 返回值：
  - 成功：`true, movedAmount`
  - 失败：`nil, reason`
- 用途：把当前选中内部储罐中的流体转移到当前选中库存槽位中的容器。

可能失败原因包括：

- `nothing selected`
- `item is full or not a fluid container`
- `tank is empty`
- `incompatible or no fluid`
- `no tank`

## 使用说明

- 这些回调同时支持原版式的一次性流体容器，以及实现了 `IFluidContainerItem` 的可重复使用容器。
- 当容器在操作后需要变成另一种物品时，结果物品会优先尝试放回库存；放不下时会直接生成到世界里。

## 示例

```lua
local robot = require("robot")

robot.select(1)
robot.selectTank(1)

print("Container level:", robot.getTankLevelInSlot())
print("Container capacity:", robot.getTankCapacityInSlot())

local ok, moved = robot.drain(1000)
print("Moved from container into tank:", ok, moved)

local ok2, moved2 = robot.fill(500)
print("Moved from tank into container:", ok2, moved2)
```

## 相关内容

- `opencomputers-tank-control`
- `opencomputers-world-fluid-container-analytics`
- `robot`
