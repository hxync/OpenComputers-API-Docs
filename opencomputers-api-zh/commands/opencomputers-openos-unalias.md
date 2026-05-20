# unalias

## 概述

删除一个或多个 shell 别名。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\unalias.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\unalias`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\unalias`

## 语法

```lua
unalias NAME...
```

## 用途

依次查找每个给出的别名名称，并通过 `shell.setAlias(name, nil)` 将其删除。如果某个别名不存在，命令会逐项报告错误，但仍继续处理后续参数。

## 参数

- `NAME...`: 一个或多个要删除的别名名称。

## 示例

```lua
unalias ll
```
删除名为 `ll` 的别名。

```lua
unalias ll la lgrep
```
在一次调用中删除多个别名。

## 备注

- 如果任意别名不存在，命令会为该参数输出 `unalias: <name>: not found`，并最终以状态码 `1` 退出。
