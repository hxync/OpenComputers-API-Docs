# opencomputers-item-inventory-control

## 概述

本页记录“物品内部库存”相关回调，例如箱包、容器类物品，或者其他能通过 OpenComputers 驱动打开其内部库存的物品。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：`inventory_controller`
- 底层实现特征：`ItemInventoryControl`

## 槽位编号

- 放置该物品内部库存的宿主槽位从 `1` 开始。
- 物品内部库存本身的槽位也从 `1` 开始。

## API

### getItemInventorySize

- 语法：`inventory_controller.getItemInventorySize(slot)`
- 返回值：
  - 成功：`number`
  - 失败：`0, "no item inventory"`
- 用途：返回指定宿主槽位中那个物品内部库存的槽位总数。

### dropIntoItemInventory

- 语法：`inventory_controller.dropIntoItemInventory(inventorySlot, slot[, count])`
- 返回值：
  - 成功：`number`
  - 失败：`0, "no item inventory"`
- 用途：把宿主库存中的物品移动到某个物品内部库存的指定槽位。

返回值是实际移动的物品数量。

### suckFromItemInventory

- 语法：`inventory_controller.suckFromItemInventory(inventorySlot, slot[, count])`
- 返回值：
  - 成功：`number`
  - 失败：`0, "no item inventory"`
- 用途：把物品内部库存指定槽位中的物品拉回宿主库存。

## 示例

```lua
local component = require("component")
local ic = component.inventory_controller

local size, reason = ic.getItemInventorySize(3)
if size == 0 and reason then
  print("Slot 3 does not contain an item inventory:", reason)
else
  print("Contained inventory size:", size)
  print("Moved into contained slot:", ic.dropIntoItemInventory(3, 1, 16))
  print("Moved out of contained slot:", ic.suckFromItemInventory(3, 1, 16))
end
```

## 相关内容

- `opencomputers-inventory-analytics`
- `opencomputers-inventory-world-control-mk2`
- `inventory_controller`
