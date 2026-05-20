# opencomputers-inventory-control

## 概述

本页记录库存感知宿主暴露出的内部物品栏选择与整理接口，例如 `robot` 组件就会提供这组回调。它们操作的是设备自己的内部物品栏，而不是相邻容器。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：通常作为 `robot` 等自带库存宿主的一部分出现
- 底层实现特征：`InventoryControl`

## 槽位编号

Lua 侧槽位编号从 `1` 开始。

- 槽位 `1` 表示第一个内部槽位。
- 传入越界槽位会抛出 `invalid slot`。

## API

### inventorySize

- 语法：`device.inventorySize()`
- 返回值：`number`
- 用途：返回宿主内部物品栏的总槽位数。

### select

- 语法：`device.select([slot])`
- 返回值：`number`
- 用途：读取或修改当前选中的槽位。

不传参数时返回当前选中槽位；传入槽位后会把它设置为新的当前选中槽位。

### count

- 语法：`device.count([slot])`
- 返回值：`number`
- 用途：返回指定槽位中的物品数量；如果省略参数，则读取当前选中槽位。

空槽位返回 `0`。

### space

- 语法：`device.space([slot])`
- 返回值：`number`
- 用途：返回指定槽位剩余可容纳的物品数量；如果省略参数，则读取当前选中槽位。

对于空槽位，通常会返回该库存允许的堆叠上限。

### compareTo

- 语法：`device.compareTo(otherSlot[, checkNBT])`
- 参数：
  - `otherSlot:number`：要比较的另一个内部槽位。
  - `checkNBT:boolean`：为 `true` 时，把 NBT 也纳入比较。
- 返回值：`boolean`
- 用途：把当前选中槽位与另一个内部槽位进行比较。

行为细节：

- 两个空槽位比较结果为 `true`。
- 一个空一个非空时比较结果为 `false`。

### transferTo

- 语法：`device.transferTo(toSlot[, amount])`
- 参数：
  - `toSlot:number`：同一库存中的目标槽位。
  - `amount:number`：最多移动的物品数量，会被限制在 `0..64`。
- 返回值：`boolean`
- 用途：把当前选中槽位中的物品移动到另一个内部槽位。

行为细节：

- 如果 `toSlot` 就是当前槽位，或者 `amount` 为 `0`，会直接返回 `true`。
- 如果目标槽位是同类物品，会尽量合并堆叠。
- 只有当整个源堆允许完整移动时，才会执行整堆交换。
- 无法完成移动时返回 `false`。

## 示例

```lua
local robot = require("robot")

print("Inventory size:", robot.inventorySize())
print("Selected slot:", robot.select())

robot.select(1)
print("Items in slot 1:", robot.count())
print("Free space in slot 1:", robot.space())
print("Slot 1 equals slot 2:", robot.compareTo(2, true))

local moved = robot.transferTo(2, 8)
print("Moved items:", moved)
```

## 相关内容

- `opencomputers-inventory-analytics`
- `opencomputers-inventory-world-control`
- `robot`
