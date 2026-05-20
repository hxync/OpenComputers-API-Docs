# opencomputers-inventory-analytics

## 概述

本页记录内部库存检查相关的回调接口。它们通常由机器人或无人机上的 `inventory_controller` 升级提供。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：`inventory_controller`
- 底层实现特征：`InventoryAnalytics`

## 槽位编号

Lua 侧库存槽位从 `1` 开始编号。

## API

### getStackInInternalSlot

- 语法：`inventory_controller.getStackInInternalSlot([slot])`
- 返回值：
  - 成功：物品描述表
  - 失败：`nil, "not enabled in config"`
- 用途：返回指定内部槽位的详细物品描述；如果省略参数，则读取当前选中槽位。

这个回调受 OpenComputers 的“物品堆检查”配置项控制。

### isEquivalentTo

- 语法：`inventory_controller.isEquivalentTo(otherSlot)`
- 返回值：`boolean`
- 用途：检查当前选中槽位与另一个内部槽位是否共享至少一个矿辞条目。

行为：

- 两个空槽位视为等价。
- 一个空一个非空则视为不等价。

### storeInternal

- 语法：`inventory_controller.storeInternal(slot, dbAddress, dbSlot)`
- 返回值：`boolean`
- 用途：把内部槽位中的物品描述复制到数据库的指定槽位。

返回值含义：

- `true` 表示目标数据库槽位原本已有数据。
- `false` 表示目标数据库槽位原本为空。

重要说明：

- 该实现默认源槽位里确实有物品，空源槽位并不是正常用法。
- 如果数据库地址无效，会直接抛出诸如 `no such component` 或 `not a database` 的错误。

### compareToDatabase

- 语法：`inventory_controller.compareToDatabase(slot, dbAddress, dbSlot[, checkNBT])`
- 返回值：`boolean`
- 用途：把内部某个槽位和数据库里保存的物品记录进行比较。

`checkNBT` 默认是 `false`。

## 示例

```lua
local component = require("component")
local robot = require("robot")
local ic = component.inventory_controller
local db = component.database

robot.select(1)

local stack, reason = ic.getStackInInternalSlot()
if stack then
  print("Selected item:", stack.name, stack.size)
else
  print("Inspection unavailable:", reason)
end

ic.storeInternal(1, db.address, 1)
print("Matches database slot 1:", ic.compareToDatabase(1, db.address, 1, true))
print("Equivalent to slot 2:", ic.isEquivalentTo(2))
```

## 相关内容

- `opencomputers-inventory-control`
- `opencomputers-world-inventory-analytics`
- `database`
