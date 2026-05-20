# process

## 概述

管理基于协程的进程、继承环境状态以及归属句柄。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\process.lua`

## 用法

```lua
local process = require("process")
```

## 接口

### process.addHandle(handle, proc)

- 说明: 把句柄登记到当前或指定进程名下，以便进程退出时自动关闭。
- 参数:
  - `handle`
  - `proc`

```lua
process.addHandle(nil, nil)
```

### process.findProcess(co)

- 说明: 把某个协程解析回它所属的进程记录。
- 参数:
  - `co`

```lua
process.findProcess(nil)
```

### process.info(levelOrThread)

- 说明: 返回进程元数据，例如路径、环境、命令名以及每进程数据表。
- 参数:
  - `levelOrThread`

```lua
process.info(nil)
```

### process.load(path, env, init, name)

- 说明: 从脚本路径或函数创建一个新的基于协程的进程。
- 参数:
  - `path`
  - `env`
  - `init`
  - `name`

```lua
process.load(nil, nil, nil, "value")
```

### process.removeHandle(handle, proc)

- 说明: 把某个句柄从进程句柄列表中移除。
- 参数:
  - `handle`
  - `proc`

```lua
process.removeHandle(nil, nil)
```

### process.running(level)

- 说明: 一个兼容性辅助函数，用于返回当前进程的路径、环境和命令名。
- 参数:
  - `level`

```lua
process.running(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
