# event

## 概述

为 OpenOS 程序提供信号分发、定时器和按条件等待事件的能力。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\event.lua`

## 用法

```lua
local event = require("event")
```

## 接口

### event.cancel(timerId)

- 说明: 按标识符取消定时器或已注册处理器。
- 参数:
  - `timerId`

```lua
event.cancel(nil)
```

### event.ignore(name, callback)

- 说明: 移除某个命名信号对应的监听器。
- 参数:
  - `name`
  - `callback`

```lua
event.ignore("value", nil)
```

### event.listen(name, callback)

- 说明: 为命名信号注册回调；如果相同监听已存在则不会重复注册。
- 参数:
  - `name`
  - `callback`

```lua
event.listen("value", nil)
```

### event.onError(message)

- 说明: 处理事件回调中抛出的异步错误。
- 参数:
  - `message`

```lua
event.onError(nil)
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

### event.pullMultiple(...)

- 说明: 等待匹配任一给定名称的下一个信号。
- 参数:
  - `...`

```lua
event.pullMultiple(nil)
```

### event.shouldInterrupt()

- 说明: 返回当前输入状态是否应该触发中断。

```lua
event.shouldInterrupt()
```

### event.shouldSoftInterrupt()

- 说明: 返回当前输入状态是否应该触发软中断。

```lua
event.shouldSoftInterrupt()
```

### event.timer(interval, callback, times)

- 说明: 创建定时器，在稍后或重复触发时调用回调。
- 参数:
  - `interval`
  - `callback`
  - `times`

```lua
event.timer(0, nil, nil)
```

## 备注

- 本页只列出模块导出的公开函数。
