# time

## 概述

测量某条 shell 命令的实际耗时和 CPU 耗时。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\time.lua`

## 语法

```lua
time [COMMAND [ARGS...]]
```

## 用途

在执行 shell 命令前后分别记录 `computer.uptime()` 与 `os.clock()`，然后按“分钟 + 秒”的形式打印 `real` 与 `cpu` 两行耗时信息。

## 参数

- `COMMAND`: 可选，要执行并计时的 shell 命令。
- `ARGS...`: 可选，原样转发给被计时命令的参数。

## 示例

```lua
time ls /bin
```
执行 `ls /bin`，然后打印实际耗时和 CPU 耗时。

```lua
time
```
不执行其他命令，直接打印接近零的计时结果。

## 备注

- 命令本身会返回被计时 shell 命令的最后退出码。
