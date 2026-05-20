# dsu

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Deep Storage Unit 集成接口。

## 可用性

- 依赖: `dsu`
- 标签: `integration-required`

## 组件名

- `deep_storage_unit`

## 主要用途

- 读取 Deep Storage Unit 的总存储容量。
- 检查当前已经存入了多少物品。
- 在配置允许物品栈检查时，读取当前存储的物品类型。

## 组件

### deep_storage_unit

`deep_storage_unit` 用于实现 MineFactory Reloaded `IDeepStorageUnit` 接口的方块。

#### `getMaxStoredCount()`

- 语法: `getMaxStoredCount(): number`
- 用途: 返回 DSU 可容纳的最大物品数量。
- 返回值:
  - 最大存储数量。

#### `getStoredCount()`

- 语法: `getStoredCount(): number`
- 用途: 返回当前已经存入的物品数量。
- 返回值:
  - 当前存储数量。
- 说明:
  - 如果当前没有存储任何物品类型，会返回 `0`。

#### `getStoredItemType()`

- 语法: `getStoredItemType(): table[, string]`
- 用途: 返回当前存储物品类型的物品栈描述。
- 返回值:
  - 启用时返回物品栈描述表。
  - 若配置未允许物品栈检查，则返回 `nil, "not enabled in config"`。
- 说明:
  - 这个接口依赖 OpenComputers 配置项 `allowItemStackInspection`。
  - 如果 DSU 当前没有存入任何物品类型，则启用检查时会返回 `nil`。

## 示例

```lua
local component = require("component")
local dsu = component.deep_storage_unit

print("Stored:", dsu.getStoredCount(), "/", dsu.getMaxStoredCount())
local stack, err = dsu.getStoredItemType()
print(stack and stack.label or err)
```

## 相关内容

- `storagedrawers`
- `vanilla`
