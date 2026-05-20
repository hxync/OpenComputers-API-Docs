# alias

## 概述

列出、查看并修改 shell 命令别名。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\alias.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\alias`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\alias`

## 语法

```lua
alias
```
```lua
alias NAME
```
```lua
alias NAME=value
```

## 用途

管理 shell 别名，用一个命令名展开成另一段命令行。不带参数时列出全部别名；只给 `name` 时查看该别名；给出 `name=value` 时更新该别名。

## 参数

- `NAME`: 要查询的别名名称。
- `NAME=value`: 别名赋值表达式。

## 选项

- `--help`: 输出简短帮助信息。

## 示例

```lua
alias
```
输出当前全部别名。

```lua
alias ll
```
输出别名 `ll` 当前对应的值。

```lua
alias ll='ls -lh'
```
把别名 `ll` 设置为执行 `ls -lh`。

```lua
alias ll editor='edit /etc/profile'
```
在一次调用中同时查询和设置多个别名。

## 备注

- 别名名称不能包含 `/`、`$`、反引号、`=`、管道、重定向符号或空白字符等 shell 元字符。
- 如果别名值里包含空格，应使用引号把它包起来，确保 shell 把它作为一个参数传入。
