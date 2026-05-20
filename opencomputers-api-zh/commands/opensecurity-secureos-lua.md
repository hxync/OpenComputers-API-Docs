# lua

## 概述

启动 Lua 解释器，或执行指定的 Lua 脚本入口文件。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\lua.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\lua`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\de_DE\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\en_US\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\fr_FR\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\ru_RU\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\zh_CN\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\lua`

## 语法

```lua
lua
```
```lua
lua FILE [ARGS...]
```

## 用途

不带参数时，命令会启动内置的 Lua shell 入口脚本。提供文件名时，它会读取该文件、去掉可能存在的 shebang，然后加载并把剩余参数一并传给脚本执行。

## 参数

- `FILE`: 可选，要执行的 Lua 脚本；提供后不会进入交互 shell。
- `ARGS...`: 可选，传给已加载脚本的额外参数。

## 示例

```lua
lua
```
启动交互式 Lua 解释器 shell。

```lua
lua /home/test.lua foo bar
```
执行 `/home/test.lua`，并把 `foo` 与 `bar` 作为脚本参数传入。

## 备注

- 交互解释器会尝试对未定义的全局名执行 `require` 解析。
- 语法错误或运行时错误会写到标准错误输出，并导致命令以失败状态退出。
