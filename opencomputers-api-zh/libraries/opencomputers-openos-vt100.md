# vt100

## 概述

TTY 写入器用来更新光标与颜色状态的最小化 VT100 转义序列解析器。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\vt100.lua`

## 用法

```lua
local vt100 = require("vt100")
```

## 接口

### vt100.parse(window)

- 说明: 从 `window.output_buffer` 中消费一个 VT100 转义序列，应用其副作用，并在解析失败时返回可打印的回退文本。
- 参数:
  - `window`: TTY 窗口状态表，包含输出缓冲、光标、GPU 代理以及已保存的属性。

```lua
local passthrough = vt100.parse(tty.window)
```

## 备注

- 引导版解析器会先处理常见的颜色与保存/恢复序列；如果遇到更高级的控制序列，再按需加载 `/lib/core/full_vt.lua`。
