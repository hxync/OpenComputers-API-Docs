# opencomputers-world-fluid-container-analytics

## 概述

本页记录对相邻库存中流体容器物品进行只读检查的回调接口。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：通常作为 `transposer` 或 `inventory_controller` 这类世界检查设备的一部分出现
- 底层实现特征：`WorldFluidContainerAnalytics`

## 通用规则

- 侧面编号用于指定相邻库存。
- 库存槽位从 `1` 开始。
- 这些回调要求在 OpenComputers 配置中启用物品堆检查。

## API

### getContainerCapacityInSlot

- 语法：`device.getContainerCapacityInSlot(side, slot)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, reason`
- 用途：返回指定相邻库存槽位中流体容器物品的总容量。

### getContainerLevelInSlot

- 语法：`device.getContainerLevelInSlot(side, slot)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, reason`
- 用途：返回指定相邻库存槽位中当前装有的流体量。

### getFluidInContainerInSlot

- 语法：`device.getFluidInContainerInSlot(side, slot)`
- 返回值：
  - 成功：流体描述表或 `nil`
  - 失败：`nil, reason`
- 用途：返回指定相邻库存槽位中容器物品的流体描述。

常见失败原因：

- `not enabled in config`
- `no inventory`
- `item is not a fluid container`

## 示例

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

local cap, reason = transposer.getContainerCapacityInSlot(sides.front, 1)
print("Container capacity:", cap, reason)

local level = transposer.getContainerLevelInSlot(sides.front, 1)
print("Container level:", level)

local fluid = transposer.getFluidInContainerInSlot(sides.front, 1)
if fluid then
  print("Fluid name:", fluid.name)
end
```

## 相关内容

- `opencomputers-fluid-container-transfer`
- `opencomputers-world-inventory-analytics`
