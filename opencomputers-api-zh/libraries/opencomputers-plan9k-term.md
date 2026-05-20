# term

## 概述

用于光标控制、行编辑和屏幕感知文本输出的终端辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\term.lua`

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

### term.clearTab()

- 说明: 清除当前光标位置上的制表位。

```lua
term.clearTab()
```

### term.clearTabs()

- 说明: 清除所有已经设置的制表位。

```lua
term.clearTabs()
```

### term.copy(row, col, height, width, destRow, destCol)

- 说明: 使用 Plan9k 终端扩展把一个矩形屏幕区域复制到另一个位置。
- 参数:
  - `row`
  - `col`
  - `height`
  - `width`
  - `destRow`
  - `destCol`

```lua
term.copy(nil, nil, nil, nil, nil, nil)
```

### term.cursorBackward(count)

- 说明: 把终端光标向左移动指定列数。
- 参数:
  - `count`

```lua
term.cursorBackward(nil)
```

### term.cursorDown(count)

- 说明: 把终端光标向下移动指定行数。
- 参数:
  - `count`

```lua
term.cursorDown(nil)
```

### term.cursorForward(count)

- 说明: 把终端光标向右移动指定列数。
- 参数:
  - `count`

```lua
term.cursorForward(nil)
```

### term.cursorUp(count)

- 说明: 把终端光标向上移动指定行数。
- 参数:
  - `count`

```lua
term.cursorUp(nil)
```

### term.disableLineWrap()

- 说明: 禁用文本到达视口边缘时的自动换行。

```lua
term.disableLineWrap()
```

### term.enableLineWrap()

- 说明: 启用文本到达视口边缘时的自动换行。

```lua
term.enableLineWrap()
```

### term.enableScroll()

- 说明: 为整个显示区域启用滚动区域。

```lua
term.enableScroll()
```

### term.eraseDown()

- 说明: 从当前光标所在行开始擦除到屏幕底部。

```lua
term.eraseDown()
```

### term.eraseEndOfLine()

- 说明: 从当前光标位置擦除到本行末尾。

```lua
term.eraseEndOfLine()
```

### term.eraseLine()

- 说明: 擦除整条当前行。

```lua
term.eraseLine()
```

### term.eraseStartOfLine()

- 说明: 从当前光标位置向前擦除到本行开头。

```lua
term.eraseStartOfLine()
```

### term.eraseUp()

- 说明: 从当前光标所在行开始擦除到屏幕顶部。

```lua
term.eraseUp()
```

### term.fill(row, col, height, width)

- 说明: 使用 Plan9k 终端扩展填充一个矩形屏幕区域。
- 参数:
  - `row`
  - `col`
  - `height`
  - `width`

```lua
term.fill(nil, nil, nil, nil)
```

### term.forceCursorPosition(row, col)

- 说明: 使用 VT100 的 `f` 定位序列设置光标位置。
- 参数:
  - `row`
  - `col`

```lua
term.forceCursorPosition(nil, nil)
```

### term.getCursor()

- 说明: 返回当前光标所在的列和行。

```lua
term.getCursor()
```

### term.getCursorBlink(enabled)

- 说明: 返回当前是否启用了光标闪烁。
- 参数:
  - `enabled`

```lua
term.getCursorBlink(nil)
```

### term.getInfo()

- 说明: 向 Plan9k 终端后端查询当前活动 GPU 和屏幕地址。

```lua
term.getInfo()
```

### term.getResolution()

- 说明: 返回当前终端的字符宽度和高度。

```lua
term.getResolution()
```

### term.gpu()

- 说明: 返回与当前 Plan9k 终端会话关联的 GPU 代理。

```lua
term.gpu()
```

### term.isAvailable()

- 说明: 返回当前系统是否拥有可用于终端输出的 GPU 与屏幕组合。

```lua
term.isAvailable()
```

### term.read(history, dobreak, hint, pwchar)

- 说明: 读取一行可编辑输入，并支持历史记录、补全提示、密码掩码和校验过滤。
- 参数:
  - `history`
  - `dobreak`
  - `hint`
  - `pwchar`

```lua
term.read(nil, nil, 0, nil)
```

### term.reset()

- 说明: 把终端恢复到默认的全屏状态，并在支持时重置颜色和分辨率。

```lua
term.reset()
```

### term.resetCursor()

- 说明: 把光标移动回左上角的起始位置。

```lua
term.resetCursor()
```

### term.restoreCursor()

- 说明: 恢复由 `saveCursor` 保存的光标位置。

```lua
term.restoreCursor()
```

### term.restoreCursorAndAttr()

- 说明: 恢复由 `saveCursorAndAttr` 保存的光标位置和文本属性。

```lua
term.restoreCursorAndAttr()
```

### term.saveCursor()

- 说明: 保存当前光标位置，供之后恢复。

```lua
term.saveCursor()
```

### term.saveCursorAndAttr()

- 说明: 保存当前光标位置以及文本属性。

```lua
term.saveCursorAndAttr()
```

### term.scrollScreen(from,to)

- 说明: 把终端显示的滚动区域限制在指定的行范围内。
- 参数:
  - `from`
  - `to`

```lua
term.scrollScreen(nil, nil)
```

### term.scrollScreenDown()

- 说明: 把显示内容向下滚动一行。

```lua
term.scrollScreenDown()
```

### term.scrollScreenUp()

- 说明: 把显示内容向上滚动一行。

```lua
term.scrollScreenUp()
```

### term.setAttr(attr)

- 说明: 应用一个文本属性模式，例如重置、高亮、闪烁或反显。
- 参数:
  - `attr`

```lua
term.setAttr(nil)
```

### term.setBackground(color)

- 说明: 使用 ANSI 调色板设置终端背景色。
- 参数:
  - `color`

```lua
term.setBackground(nil)
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

### term.setCursorPosition(row,col)

- 说明: 使用 VT100 的 `H` 定位序列把光标设置到指定行列。
- 参数:
  - `row`
  - `col`

```lua
term.setCursorPosition(nil, nil)
```

### term.setForeground(color)

- 说明: 使用 ANSI 调色板设置终端前景色。
- 参数:
  - `color`

```lua
term.setForeground(nil)
```

### term.setResolution(height,width)

- 说明: 通过 Plan9k 终端扩展请求新的终端分辨率。
- 参数:
  - `height`
  - `width`

```lua
term.setResolution(nil, nil)
```

### term.tab()

- 说明: 在当前光标位置设置一个制表位。

```lua
term.tab()
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
