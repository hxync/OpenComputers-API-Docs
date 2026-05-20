# betterstorage

## 概述

本页说明 OpenComputers 对 BetterStorage 箱筐的集成接口。crate 箱筐会作为 Lua 组件暴露，可用于列出当前内容物；在较新的 BetterStorage API 版本中，还能读取箱筐容量。

## 可用性

- 依赖: `betterstorage`
- 标签: `integration-required`

## 组件名

- `crate`

## 主要用途

- 列出箱筐中当前存放的全部物品堆。
- 为基于箱筐的仓储系统编写库存浏览器。
- 在较新的 BetterStorage API 下估算剩余可用存储空间。

## 组件

OpenComputers 同时兼容 BetterStorage 的两套 crate API，因为不同版本的模组提供的接口并不完全相同。不过它们对 Lua 暴露的组件名都是同一个。

### crate

#### `getContents()`

- 语法: `getContents(): table`
- 用途: 返回当前箱筐内容物的数组，每一项都是一个物品堆表。
- 返回值:
  - 由物品堆描述表组成的数组快照。
- 说明:
  - 返回的物品堆结构遵循 OpenComputers 常见的物品表格式，通常会包含物品名、显示名、damage 值、数量，以及可用时的 NBT 相关信息。
  - `getContents()` 返回的是当前时刻的内容快照，不是可实时刷新的迭代器。
  - 返回顺序取决于箱筐内部维护的内容列表，不应把它当成稳定排序结果。

#### `getCapacity()`

- 语法: `getCapacity(): number`
- 用途: 返回该箱筐可以容纳的物品槽位数量。
- 返回值:
  - 箱筐容量。
- 说明:
  - 这个接口只存在于较新的 BetterStorage API 版本。
  - 如果你的程序要兼容多个整合包版本，调用前最好先判断该方法是否存在。

#### 示例

```lua
local component = require("component")
local crate = component.crate

if crate.getCapacity then
  print("Capacity:", crate.getCapacity())
end

for index, stack in ipairs(crate.getContents()) do
  print(index, stack.label or stack.name, stack.size)
end
```

## 相关内容

- `storagedrawers`
- `opencomputers-inventory-controller`
