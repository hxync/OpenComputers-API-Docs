# tty

## 概述

位于 `term` 和 `io` 之下的终端视口、光标、键盘和输出流辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\tty.lua`

## 用法

```lua
local tty = require("tty")
```

## 接口

### tty.bind(gpu)

- 说明: 把终端输出绑定到指定 GPU 代理，并刷新缓存的视口信息。
- 参数:
  - `gpu`: 要作为活动终端后端使用的 GPU 代理。

```lua
tty.bind(component.gpu)
```

### tty.clear()

- 说明: 通过滚动清空整个视口，并把逻辑光标重置到左上角。

```lua
tty.clear()
```

### tty.getCursor()

- 说明: 返回视口内当前逻辑光标所在的列和行。

```lua
local x, y = tty.getCursor()
```

### tty.getViewport()

- 说明: 返回视口宽高、偏移量以及当前光标位置。

```lua
local w, h, dx, dy, x, y = tty.getViewport()
```

### tty.gpu()

- 说明: 返回当前绑定到终端窗口的 GPU 代理。

```lua
local gpu = tty.gpu()
```

### tty.isAvailable()

- 说明: 返回终端当前是否有已绑定 GPU，且该 GPU 连接到了屏幕。

```lua
if tty.isAvailable() then tty.clear() end
```

### tty.keyboard()

- 说明: 解析与当前活动屏幕关联的键盘地址；如果没有则回退到主键盘。

```lua
local keyboard = tty.keyboard()
```

### tty.screen()

- 说明: 返回当前绑定 GPU 所连接的屏幕地址。

```lua
local screen = tty.screen()
```

### tty.setCursor(x, y)

- 说明: 更新 `tty.window` 中记录的逻辑光标位置。
- 参数:
  - `x`: 视口内的目标列。
  - `y`: 视口内的目标行。

```lua
tty.setCursor(1, 1)
```

### tty.setViewport(width, height, dx, dy, x, y)

- 说明: 设置自定义视口矩形，以及该区域内的初始光标位置。
- 参数:
  - `width`: 视口宽度，单位为字符。
  - `height`: 视口高度，单位为字符。
  - `dx`: 可选，相对于屏幕原点的 X 偏移。
  - `dy`: 可选，相对于屏幕原点的 Y 偏移。
  - `x`: 可选，初始光标列，默认为 `1`。
  - `y`: 可选，初始光标行，默认为 `1`。

```lua
tty.setViewport(40, 10, 0, 0, 1, 1)
```

## 备注

- 这个模块围绕 `tty.window` 工作，它会跟踪当前 GPU、视口、光标位置以及缓冲中的 VT100 输出状态。
