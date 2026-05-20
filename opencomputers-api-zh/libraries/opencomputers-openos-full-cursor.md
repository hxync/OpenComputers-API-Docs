# core_cursor

## 概述

为光标补充鼠标定位、Tab 补全和横向不换行编辑能力的延迟加载扩展。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_cursor.lua`

## 用法

```lua
local core_cursor = require("core/cursor")  -- loads horizontal helpers lazily
```

## 接口

### core_cursor.tab(cursor)

- 说明: 循环使用光标 `hint` 回调提供的补全候选；按 Shift+Tab 会反向切换。
- 参数:
  - `cursor`: 带有 `hint` 字段的光标对象；该字段需要返回补全候选。

```lua
core_cursor.tab(cursor)
```

### core_cursor.touch(cursor, gx, gy)

- 说明: 把编辑光标移动到最接近被触摸屏幕单元的位置。
- 参数:
  - `cursor`: 要重新定位的光标对象。
  - `gx`: 全局屏幕坐标系中的触摸 X 坐标。
  - `gy`: 全局屏幕坐标系中的触摸 Y 坐标。

```lua
core_cursor.touch(cursor, x, y)
```

## 备注

- 这些辅助函数会在基础光标模块通过 `package.delay` 按需加载 `/lib/core/full_cursor.lua` 时挂接进去。
