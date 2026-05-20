# shutdown

## 概述

立即清空终端并关闭 Plan9k 机器。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\shutdown.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\shutdown`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\shutdown`

## 语法

```lua
shutdown
```

## 用途

清空当前终端，然后立即调用 `computer.shutdown()`。

## 示例

```lua
shutdown
```
立即关闭 Plan9k 机器。
