# sh

## 概述

OpenOS shell 的执行辅助库，负责别名展开、变量替换、管道执行和补全钩子。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\sh.lua`

## 用法

```lua
local sh = require("sh")
```

## 接口

### sh.execute(env, command, ...)

- 说明: 解析并执行一条 shell 命令行，同时更新 shell 记录的上一次退出码。
- 参数:
  - `env`: 在合适时传给子进程的环境表。
  - `command`: 要解析并执行的命令行字符串。
  - `...`: 附加参数，会传给最终管道阶段或直接子进程。

```lua
sh.execute(_ENV, "echo $HOME")
```

### sh.expand(value)

- 说明: 在一个已经分词的文本片段中展开 `$HOME`、`$?`、`${NAME}` 之类的 shell 变量。
- 参数:
  - `value`: 要展开的单个文本片段。

```lua
local expanded = sh.expand("last=$?")
```

### sh.getLastExitCode()

- 说明: 返回 `sh.execute` 最近一次记录的 shell 风格数字退出码。

```lua
local ec = sh.getLastExitCode()
```

### sh.hintHandler(full_line, cursor)

- 说明: 根据一条命令行和光标位置返回补全候选。
- 参数:
  - `full_line`: 当前正在编辑的完整命令行。
  - `cursor`: `full_line` 中的 1 基光标位置。

```lua
local hints = sh.hintHandler('ls /et', 7)
```

## 备注

- 这个较小的引导模块负责简单执行和展开；更高级的解析、通配和重定向辅助逻辑会从 `full_sh` 按需加载。
