# watch

## 概述

循环清屏并每秒重新执行一次指定命令。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\watch.lua`

## 语法

```lua
watch COMMAND [ARGS...]
```

## 用途

先清空终端，再用给定参数启动目标命令，等待该子进程结束，休眠一秒，然后无限重复这一流程。

## 参数

- `COMMAND`: 要循环执行的程序路径或可执行名称。
- `ARGS...`: 每次重新执行时都会传入的可选参数。

## 示例

```lua
watch ps
```
每秒刷新一次进程列表。

```lua
watch ls /mnt
```
持续清屏并显示 `/mnt` 的内容。

## 备注

- 刷新间隔固定写死为 1 秒。
