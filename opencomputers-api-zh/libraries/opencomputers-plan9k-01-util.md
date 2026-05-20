# allocator

## 概述

被多个 Plan9k 内核式模块复用的内部编号分配辅助对象。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\01_util.lua`

## 用法

```lua
-- internal Plan9k utility allocator returned by getAllocator()
```

## 接口

### allocator.get()

- 说明: 分配下一个空闲数字条目，并返回为该槽位保存的可复用表对象。

```lua
local entry = allocator:get()
```

### allocator.unset(e)

- 说明: 把一个先前分配出去的条目释放回分配器的空闲链表。
- 参数:
  - `e`: 先前由 `allocator:get()` 返回的已分配条目对象。

```lua
allocator:unset(entry)
```

## 备注

- 本页只列出模块导出的公开函数。
