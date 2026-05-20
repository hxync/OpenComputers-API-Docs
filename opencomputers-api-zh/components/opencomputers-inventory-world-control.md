# opencomputers-inventory-world-control

## 概述

本页记录 `robot` 组件以及类似代理设备使用的经典“面向世界”的库存交互回调。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：通常作为 `robot` 或代理类组件的一部分出现
- 底层实现特征：`InventoryWorldControl`

## API

### compare

- 语法：`device.compare(side[, fuzzy])`
- 返回值：`boolean`
- 用途：把指定侧面的方块与当前选中槽位中的物品进行比较。

工作方式：

- 只有当选中物品是方块物品时才可能成功。
- 物品 ID 必须与目标方块匹配。
- 除非 `fuzzy` 为 `true`，否则元数据也必须匹配。

### drop

- 语法：`device.drop(side[, count])`
- 返回值：
  - 成功：`true`
  - 失败：`false` 或 `false, "inventory full"`
- 用途：把当前选中槽位中的物品朝指定侧面丢出。

行为：

- 如果指定方向有可用且允许访问的相邻库存，会优先尝试插入那个库存。
- 否则会把物品作为掉落物抛到世界中。
- `count` 默认是 `64`。

### suck

- 语法：`device.suck(side[, count])`
- 返回值：
  - 成功：`number`
  - 失败：`false`
- 用途：从指定侧面吸取物品。

行为：

- 先尝试从相邻库存提取。
- 如果提取不到，再尝试拾取该侧面的掉落物实体。
- 成功时返回实际收集到的物品数量。

## 使用说明

- `drop` 和 `suck` 都会按照 OpenComputers 的常规动作延迟暂停执行。
- 当什么也没有吸到时，`suck` 返回的是 `false`，不是 `0`。

## 示例

```lua
local robot = require("robot")
local sides = require("sides")

robot.select(1)
print("Front block matches selected stack:", robot.compare(sides.front, true))

local ok, reason = robot.drop(sides.front, 16)
print("Drop result:", ok, reason)

local taken = robot.suck(sides.front, 8)
print("Items collected:", taken)
```

## 相关内容

- `opencomputers-inventory-world-control-mk2`
- `opencomputers-inventory-control`
- `robot`
