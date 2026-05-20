# opencomputers-world-inventory-analytics

## 概述

本页记录针对相邻库存的只读检查与元数据分析回调接口。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：常由 `transposer` 和 `inventory_controller` 提供
- 底层实现特征：`WorldInventoryAnalytics`

## 通用规则

- 侧面编号用于指定相邻库存。
- 槽位编号从 `1` 开始。
- 部分回调要求在配置中启用物品堆检查。
- 无效数据库地址会直接抛出诸如 `no such component` 或 `not a database` 的错误。

## API

### getInventorySize

- 语法：`device.getInventorySize(side)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "no inventory"`
- 用途：返回相邻库存的槽位总数。

### getSlotStackSize

- 语法：`device.getSlotStackSize(side, slot)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "no inventory"`
- 用途：返回相邻库存某个槽位中的物品数量。

### getSlotMaxStackSize

- 语法：`device.getSlotMaxStackSize(side, slot)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, "no inventory"`
- 用途：返回相邻库存某个槽位中当前物品的最大堆叠数量。

空槽位返回 `0`。

### compareStacks

- 语法：`device.compareStacks(side, slotA, slotB[, checkNBT])`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, "no inventory"`
- 用途：检查同一个相邻库存中的两个槽位是否为同一种物品。

### compareStackToDatabase

- 语法：`device.compareStackToDatabase(side, slot, dbAddress, dbSlot[, checkNBT])`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, "no inventory"`
- 用途：把相邻库存中的某个槽位与数据库中的一条记录进行比较。

### areStacksEquivalent

- 语法：`device.areStacksEquivalent(side, slotA, slotB)`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, "no inventory"`
- 用途：基于矿辞等价关系比较相邻库存中的两个槽位。

### setStackDisplayName

- 语法：`device.setStackDisplayName(side, slot, label)`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, "no inventory"`
- 用途：为相邻库存中的某个物品设置或清除显示名称。

如果传入空字符串或只包含空白字符的 `label`，会清除已有自定义名称。

### getStackInSlot

- 语法：`device.getStackInSlot(side, slot)`
- 返回值：
  - 成功：物品描述表
  - 失败：`nil, reason`
- 用途：返回相邻库存某个槽位的详细物品描述。

### getAllStacks

- 语法：`device.getAllStacks(side)`
- 返回值：
  - 成功：用于批量访问堆叠信息的 userdata 包装对象
  - 失败：`nil, reason`
- 用途：返回相邻库存中全部槽位的整体视图。

### getInventoryName

- 语法：`device.getInventoryName(side)`
- 返回值：
  - 成功：`string`
  - 失败：`nil, reason`
- 用途：返回相邻库存来源的标识名称。

常见结果：

- 方块库存时返回方块的未本地化名
- 实体库存时返回实体字符串 ID

### store

- 语法：`device.store(side, slot, dbAddress, dbSlot)`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, "no inventory"`
- 用途：把相邻库存某个槽位的物品信息复制到数据库槽位中。

返回值含义：

- `true` 表示目标数据库槽位原本已有数据。
- `false` 表示目标数据库槽位原本为空。

## 常见检查失败

受检查配置控制的回调还可能返回：

- `nil, "not enabled in config"`

## 示例

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer
local db = component.database

print("Front inventory size:", transposer.getInventorySize(sides.front))
print("Slot 1 stack size:", transposer.getSlotStackSize(sides.front, 1))
print("Slot 1 equals slot 2:", transposer.compareStacks(sides.front, 1, 2, true))

local stack = transposer.getStackInSlot(sides.front, 1)
if stack then
  print("Slot 1 item:", stack.name)
end

transposer.store(sides.front, 1, db.address, 1)
print("Database match:", transposer.compareStackToDatabase(sides.front, 1, db.address, 1, true))
```

## 相关内容

- `opencomputers-inventory-transfer`
- `opencomputers-world-fluid-container-analytics`
- `database`
