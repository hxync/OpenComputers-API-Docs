# term

## 概述

用于光标控制、行编辑和屏幕感知文本输出的终端辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\term.lua`

## 用法

```lua
local term = require("term")
```

## 接口

### term.bind(gpu, window)

- 说明: 把终端输出绑定到指定 GPU，并可选限制在某个窗口对象内。
- 参数:
  - `gpu`
  - `window`

```lua
term.bind(nil, nil)
```

### term.clearLine()

- 说明: 擦除当前行，并把光标移回该行第一列。

```lua
term.clearLine()
```

### term.getCursorBlink()

- 说明: 返回当前是否启用了光标闪烁。

```lua
term.getCursorBlink()
```

### term.getGlobalArea()

- 说明: 返回当前终端视口在屏幕上的绝对区域。

```lua
term.getGlobalArea()
```

### term.pull(...)

- 说明: 在维持终端光标状态更新的同时等待输入或其他事件。
- 参数:
  - `...`

```lua
term.pull(nil)
```

### term.read(history, dobreak, hint, pwchar, filter)

- 说明: 读取一行可编辑输入，并支持历史记录、补全提示、密码掩码和校验过滤。
- 参数:
  - `history`
  - `dobreak`
  - `hint`
  - `pwchar`
  - `filter`

```lua
term.read(nil, nil, 0, nil, nil)
```

### term.setCursorBlink(enabled)

- 说明: 开启或关闭光标闪烁。
- 参数:
  - `enabled`

```lua
term.setCursorBlink(nil)
```

### term.write(value, wrap)

- 说明: 向终端写入文本，并可选按视口宽度自动换行。
- 参数:
  - `value`
  - `wrap`

```lua
term.write(nil, nil)
```

## 备注

- 不同系统暴露的终端接口子集并不完全相同，但它们都围绕光标状态、交互式读取以及通过当前屏幕和 GPU 进行输出这三个核心能力展开。
