# set

## 概述

查看环境变量，或为 shell 设置环境值。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\set.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\set`

## 语法

```lua
set [VARIABLE]=[VALUE]
```

## 用途

不带参数时，命令会按 `key='value'` 的格式输出当前环境。形如 `NAME=value` 的参数会更新环境变量；不带 `=` 的参数会被写入位置参数。

## 参数

- `NAME=value`: 设置一个环境变量。
- `ARG`: 写入位置参数值，对应 `$1`、`$2` 等。

## 示例

```lua
set
```
显示当前全部环境变量。

```lua
set example="Hello World"
```
把 `example` 设置为 `Hello World`。

## 备注

- 当脚本第一次遇到不带 `=` 的位置参数时，会先清空已有的位置参数，再重新按顺序写入。
