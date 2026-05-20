# shell

## 概述

用于 shell 路径解析、别名管理和参数解析的辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\shell.lua`

## 用法

```lua
local shell = require("shell")
```

## 接口

### shell.getAlias(alias)

- 说明: 查询 shell 别名的值。
- 参数:
  - `alias`

```lua
shell.getAlias(nil)
```

### shell.getShell()

- 说明: 加载并返回当前配置的 shell 入口。

```lua
shell.getShell()
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

### shell.prime()

- 说明: 确保当前进程的 shell 别名表和变量表已经初始化。

```lua
shell.prime()
```

### shell.resolve(path, ext)

- 说明: 结合当前目录和 `PATH` 解析路径或命令名。
- 参数:
  - `path`
  - `ext`

```lua
shell.resolve(nil, nil)
```

### shell.setAlias(alias, value)

- 说明: 设置或清除 shell 别名。
- 参数:
  - `alias`
  - `value`

```lua
shell.setAlias(nil, nil)
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
