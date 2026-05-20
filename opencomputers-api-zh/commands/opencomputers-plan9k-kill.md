# kill

## 概述

按 PID 向 Plan9k 进程发送 `kill` 信号。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\kill.lua`

## 语法

```lua
kill PID
```

## 用途

把唯一参数转换成数字 PID，然后调用 `os.kill(pid, "kill")`。如果 PID 非法，或者信号发送失败，命令会直接抛出错误。

## 参数

- `PID`: 要终止的数字进程标识符。

## 示例

```lua
kill 42
```
请求终止进程 `42`。

## 备注

- 这个实现始终发送固定的 `kill` 信号，并不支持切换成其他信号名。
