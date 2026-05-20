# core_cursor

## 概述

供 OpenOS 终端行输入使用的可编辑光标状态机。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\cursor.lua`

## 用法

```lua
local core_cursor = require("core/cursor")
```

## 接口

### core_cursor.new(base, index)

- 说明: 创建或重新初始化一个光标对象，并可选地扩展自定义基表或替代父行为。
- 参数:
  - `base`: 可选，已有的表；光标字段和方法会写入其中。
  - `index`: 可选，作为元表 `__index` 链使用的备用父表。

```lua
local cursor = require("core/cursor").new()
```

### core_cursor.read(cursor)

- 说明: 运行交互输入循环，直到得到一整行输入、类似 EOF 的结束状态，或中断事件为止。
- 参数:
  - `cursor`: 由 `core_cursor.new` 创建的光标对象。

```lua
local line, reason = require("core/cursor").read(cursor)
```

## 备注

- 这个模块为 `tty.stream.read` 提供历史记录、插入删除、光标移动以及剪贴板输入处理能力。
