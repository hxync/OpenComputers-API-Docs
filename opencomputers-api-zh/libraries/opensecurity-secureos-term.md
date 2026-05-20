# term

## 概述

用于光标控制、行编辑和屏幕感知文本输出的终端辅助库。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\term.lua`

## 用法

```lua
local term = require("term")
```

## 接口

### term.clear()

- 说明: 清空整个终端区域，并把光标重置到左上角。

```lua
term.clear()
```

### term.clearLine()

- 说明: 擦除当前行，并把光标移回该行第一列。

```lua
term.clearLine()
```

### term.getCursor()

- 说明: 返回当前光标所在的列和行。

```lua
term.getCursor()
```

### term.getCursorBlink()

- 说明: 返回当前是否启用了光标闪烁。

```lua
term.getCursorBlink()
```

### term.isAvailable()

- 说明: 返回当前系统是否拥有可用于终端输出的 GPU 与屏幕组合。

```lua
term.isAvailable()
```

### term.isWide(x, y)

- 说明: 检查屏幕某个单元格是否属于宽字符。
- 参数:
  - `x`
  - `y`

```lua
term.isWide(nil, nil)
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

### term.reset()

- 说明: 把终端恢复到默认的全屏状态，并在支持时重置颜色和分辨率。

```lua
term.reset()
```

### term.setCursor(col, row)

- 说明: 把光标移动到指定列和行。
- 参数:
  - `col`
  - `row`

```lua
term.setCursor(nil, nil)
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
