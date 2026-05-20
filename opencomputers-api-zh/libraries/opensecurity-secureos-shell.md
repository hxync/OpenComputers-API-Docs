# shell

## 概述

用于 shell 路径解析、别名管理和参数解析的辅助库。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\shell.lua`

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

### shell.getAlias(alias)

- 说明: 查询 shell 别名的值。
- 参数:
  - `alias`

```lua
shell.getAlias(nil)
```

### shell.getPath()

- 说明: 返回当前的 `PATH` 环境变量。

```lua
shell.getPath()
```

### shell.getWorkingDirectory()

- 说明: 返回当前工作目录；未设置时默认是 `/`。

```lua
shell.getWorkingDirectory()
```

### shell.parse(...)

- 说明: 把命令行拆分为位置参数，以及短选项/长选项表。
- 参数:
  - `...`

```lua
shell.parse(nil)
```

### shell.resolve(path, ext)

- 说明: 结合当前目录和 `PATH` 解析路径或命令名。
- 参数:
  - `path`
  - `ext`

```lua
shell.resolve(nil, nil)
```

### shell.resolveAlias(command, args)

- 说明: 在继续执行前，先通过别名表展开命令的第一个词。
- 参数:
  - `command`
  - `args`

```lua
shell.resolveAlias(nil, nil)
```

### shell.setAlias(alias, value)

- 说明: 设置或清除 shell 别名。
- 参数:
  - `alias`
  - `value`

```lua
shell.setAlias(nil, nil)
```

### shell.setPath(value)

- 说明: 替换当前的 `PATH` 环境变量。
- 参数:
  - `value`

```lua
shell.setPath(nil)
```

### shell.setWorkingDirectory(dir)

- 说明: 在确认目录存在后切换当前工作目录。
- 参数:
  - `dir`

```lua
shell.setWorkingDirectory(nil)
```

## 备注

- OpenOS 会先暴露一套较小的引导期 shell API，随后在完整模块加载后再补上命令执行和 `PATH` 管理等附加能力。
