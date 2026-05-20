# thread

## 概述

围绕基于协程的 OpenOS 进程构建的线程式包装器，支持事件感知的挂起与等待。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\thread.lua`

## 用法

```lua
local thread = require("thread")
```

## 接口

### thread.create(fp, ...)

- 说明: 围绕一个 Lua 函数启动新的受管线程，并立即返回线程句柄。
- 参数:
  - `fp`: 作为线程主体执行的函数。
  - `...`: 在线程首次恢复执行时传给线程函数的参数。

```lua
local worker = thread.create(function(name) print(name) end, 'job-1')
```

### thread.current()

- 说明: 返回与当前运行进程栈对应的线程句柄；如果没有则返回空值。

```lua
local self = thread.current()
```

### thread.waitForAll(threads, timeout)

- 说明: 阻塞等待列表中的所有线程停止运行，或直到超时。
- 参数:
  - `threads`: 由 `thread.create` 返回的线程句柄数组。
  - `timeout`: 可选，最多等待的秒数。

```lua
thread.waitForAll(workers, 5)
```

### thread.waitForAny(threads, timeout)

- 说明: 阻塞等待列表中任意一个线程停止运行，或直到超时。
- 参数:
  - `threads`: 由 `thread.create` 返回的线程句柄数组。
  - `timeout`: 可选，最多等待的秒数。

```lua
thread.waitForAny(workers, 1)
```

## 备注

- 本页只列出模块导出的公开函数。
