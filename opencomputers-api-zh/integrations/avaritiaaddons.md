# avaritiaaddons

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Avaritia Addons 集成接口。当前集成主要围绕 Extreme Autocrafter 的幽灵配方网格。

## 可用性

- 依赖: `avaritiaaddons`
- 标签: `integration-required`

## 组件名

- `extreme_autocrafter`

## 主要用途

- 读取 Extreme Autocrafter 中保存的幽灵配方物品。
- 把数据库中的物品写入自动合成器的幽灵配方槽。
- 不打开机器 GUI 也能自动化设置配方。

## 组件

### extreme_autocrafter

`extreme_autocrafter` 用于 Avaritia Addons 的 Extreme Autocrafter 方块。

#### `getGhostItem(slot)`

- 语法: `getGhostItem(slot: number): table`
- 用途: 返回某个配方槽里配置的幽灵物品。
- 参数:
  - `slot: number`
    幽灵槽编号，范围是 `0` 到 `80`。
- 返回值:
  - 该槽位中的物品栈描述表。
- 说明:
  - 超出 `0` 到 `80` 的槽位会触发参数错误。
  - 驱动会把这个编号映射到自动合成器内部的幽灵库存区。

#### `setGhostItem(slot, database, databaseSlot[, size])`

- 语法: `setGhostItem(slot: number, database: string, databaseSlot: number[, size: number]): boolean`
- 用途: 把 OpenComputers 数据库中的物品复制到某个幽灵配方槽。
- 参数:
  - `slot: number`
    幽灵槽编号，范围是 `0` 到 `80`。
  - `database: string`
    数据库组件地址。
  - `databaseSlot: number`
    从 `1` 开始的数据库槽位编号。
  - `size: number`
    可选写入数量，默认是 `1`。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 如果数据库槽为空，或者请求数量小于 `1`，则这个槽位会被视为清空。
  - 复制后的数量会被限制在源物品允许的最大堆叠数以内。
  - 组件地址错误或目标组件不是数据库时，会触发参数错误。

## 示例

```lua
local component = require("component")
local crafter = component.extreme_autocrafter
local db = component.database

crafter.setGhostItem(0, db.address, 1, 1)
local ghost = crafter.getGhostItem(0)
print(ghost and ghost.label)
```

## 相关内容

- `vanilla`
- `appeng`
