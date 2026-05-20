# dmesg

## 概述

持续打印传入事件，直到被中断。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\dmesg.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\dmesg`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\dmesg`

## 语法

```lua
dmesg [EVENT...]
```

## 用途

监听全部事件，或者只监听指定名称的事件，并为每条事件元组附带时间戳后输出。命令会一直运行，直到收到 `interrupted` 信号，通常就是按下 `Ctrl-C`。

## 参数

- `EVENT...`: 可选，要过滤的事件名，例如 `touch`、`key_down` 或 `modem_message`。

## 示例

```lua
dmesg
```
持续打印所有传入事件，直到当前会话被中断。

```lua
dmesg touch key_down
```
只显示 `touch`、`key_down`，以及必然会监听的 `interrupted` 事件。

## 备注

- 在交互式终端里，事件名、地址和负载字段会用不同颜色显示。
- 即使提供了过滤条件，命令也始终会额外监听 `interrupted`，以便能够正常退出。
