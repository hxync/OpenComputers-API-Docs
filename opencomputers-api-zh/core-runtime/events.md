# events

## 概述

OpenComputers 中的事件处理通常围绕 `computer.pushSignal`、`computer.pullSignal`、OpenOS 的 `event` 库以及组件回调发出的信号展开。

## 用法

```lua
local event = require("event")
local name, address, x, y = event.pull("touch")
```

## 备注

- 信号可能来自硬件、组件回调，或者 `computer.pushSignal` 这样的软件调用。
- OpenOS 通过 `event` 库对底层信号处理进行了封装。
- 如果某个组件会发出特定信号，本套文档会在对应组件页面中单独说明。
