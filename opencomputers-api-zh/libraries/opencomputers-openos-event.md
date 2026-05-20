# event

## 概述

为 OpenOS 程序提供信号分发、定时器和按条件等待事件的能力。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\event.lua`

## 用法

```lua
local event = require("event")
```

## 接口

### event.listen(name, callback)

- 说明: 为命名信号注册回调；如果相同监听已存在则不会重复注册。
- 参数:
  - `name`
  - `callback`

```lua
event.listen("value", nil)
```

### event.pull(...)

- 说明: 等待下一个信号，并可按信号名或超时时间过滤。
- 参数:
  - `...`

```lua
event.pull(nil)
```

### event.pullFiltered(...)

- 说明: 等待被自定义过滤函数接受的下一个信号。
- 参数:
  - `...`

```lua
event.pullFiltered(nil)
```

### event.register(key, callback, interval, times, opt_handlers)

- 说明: 注册一个底层事件处理器，并可指定触发间隔和重复次数。
- 参数:
  - `key`
  - `callback`
  - `interval`
  - `times`
  - `opt_handlers`

```lua
event.register(nil, nil, 0, nil, nil)
```

## 备注

- 本页只列出模块导出的公开函数。
