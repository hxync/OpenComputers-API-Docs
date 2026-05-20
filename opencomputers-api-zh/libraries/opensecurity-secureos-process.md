# process

## 概述

SecureOS 的协程进程辅助库，用于加载脚本、继承环境和查询进程元数据。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\process.lua`

## 用法

```lua
local process = require("process")
```

## 接口

### process.info(levelOrThread)

- 说明: 返回当前进程、某个祖先进程层级，或显式协程线程对应的进程元数据。
- 参数:
  - `levelOrThread`

```lua
process.info(nil)
```

### process.install(path, name)

- 说明: 把 SecureOS 的进程记账逻辑安装到当前协程中，并用进程感知包装器替换 `coroutine.create` 和 `load`。
- 参数:
  - `path`
  - `name`

```lua
process.install(nil, "value")
```

### process.load(path, env, init, name)

- 说明: 把一个脚本路径加载为新的进程协程，并按需要继承或覆盖环境表。
- 参数:
  - `path`
  - `env`
  - `init`
  - `name`

```lua
process.load(nil, nil, nil, "value")
```

### process.running(level)

- 说明: 兼容性辅助函数，用来返回当前进程的路径、环境和命令名。
- 参数:
  - `level`

```lua
process.running(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
