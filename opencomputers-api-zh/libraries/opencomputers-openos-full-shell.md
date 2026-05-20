# shell

## 概述

用于 shell 路径解析、别名管理和参数解析的辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_shell.lua`

## 用法

```lua
local shell = require("shell")
```

## 接口

### shell.aliases()

- 说明: 遍历当前进程持有的别名表。

```lua
shell.aliases()
```

### shell.execute(command, env, ...)

- 说明: 通过当前配置的 shell 入口执行命令字符串，并返回子进程结果。
- 参数:
  - `command`
  - `env`
  - `...`

```lua
shell.execute(nil, nil, nil)
```

### shell.getPath()

- 说明: 返回当前的 `PATH` 环境变量。

```lua
shell.getPath()
```

### shell.setPath(value)

- 说明: 替换当前的 `PATH` 环境变量。
- 参数:
  - `value`

```lua
shell.setPath(nil)
```

## 备注

- OpenOS 会先暴露一套较小的引导期 shell API，随后在完整模块加载后再补上命令执行和 `PATH` 管理等附加能力。
