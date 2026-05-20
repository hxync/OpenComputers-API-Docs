# opencomputers-inventory-world-control-mk2

## 概述

本页记录 `inventory_controller` 提供的“精确槽位级”外部库存操作回调。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：`inventory_controller`
- 底层实现特征：`InventoryWorldControlMk2`

## 槽位编号

所有库存槽位参数都从 `1` 开始。

## API

### dropIntoSlot

- 语法：`inventory_controller.dropIntoSlot(facing, slot[, count[, fromSide]])`
- 返回值：
  - 成功：`true`
  - 失败：`false, "inventory full/invalid slot"` 或 `nil, "no inventory"`
- 用途：把当前选中内部槽位中的物品，插入到相邻库存的某一个精确槽位。

参数：

- `facing:number`：目标相邻库存所在方向。
- `slot:number`：目标库存中的精确槽位。
- `count:number`：最多插入多少个物品，默认 `64`。
- `fromSide:number`：可选，指定从目标库存的哪一侧进行插入。默认是 `facing` 的对面。

### suckFromSlot

- 语法：`inventory_controller.suckFromSlot(facing, slot[, count[, fromSide]])`
- 返回值：
  - 成功：`number`
  - 失败：`false` 或 `nil, "no inventory"`
- 用途：从相邻库存的某一个精确槽位中提取物品到宿主库存。

参数含义与 `dropIntoSlot` 相同。

## 示例

```lua
local component = require("component")
local robot = require("robot")
local sides = require("sides")
local ic = component.inventory_controller

robot.select(1)

local ok, reason = ic.dropIntoSlot(sides.front, 3, 8)
print("Drop into slot result:", ok, reason)

local moved = ic.suckFromSlot(sides.front, 5, 16)
print("Items taken from external slot:", moved)
```

## 相关内容

- `opencomputers-inventory-world-control`
- `opencomputers-world-inventory-analytics`
- `inventory_controller`
