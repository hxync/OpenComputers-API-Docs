# pipe

## 概述

用于把 OpenOS 进程通过读写流连接起来的基于协程的管道辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\pipe.lua`

## 用法

```lua
local pipe = require("pipe")
```

## 接口

### pipe.buildPipeChain(progs)

- 说明: 把一组已经加载的进程线程连接成 `prog1 | prog2 | ...` 形式的管道链。
- 参数:
  - `progs`: 按管道顺序排列的进程根协程数组。

```lua
pipe.buildPipeChain({producer, consumer})
```

### pipe.createCoroutineStack(root, env, name)

- 说明: 把一个根协程或函数包装成可安全跨越管道边界让出执行权的协程处理器。
- 参数:
  - `root`: 要包装的已有进程协程，或函数入口。
  - `env`: 可选，当 `root` 是函数并需要按进程加载时使用的环境表。
  - `name`: 可选，把函数根加载为进程时使用的进程名。

```lua
local stack = pipe.createCoroutineStack(threadRoot)
```

### pipe.popen(prog, mode, env)

- 说明: 在控制流后端启动一个命令，并返回一个可对其读写的缓冲句柄。
- 参数:
  - `prog`: 要执行的 shell 命令行。
  - `mode`: 取值为 `"r"`（读取命令输出）或 `"w"`（写入命令输入）。
  - `env`: 可选，传给新建 shell 执行环境的环境表。

```lua
local handle = pipe.popen("ls /etc", "r")
```

## 备注

- 这个实现会构建专用协程栈，从而让自然让出执行权和管道读取在串联命令之间正确共存。
