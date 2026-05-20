# shutdown

## 概述

按延迟计划执行 SecureOS 关机或重启，也可立即执行。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\shutdown.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\shutdown`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\shutdown`

## 语法

```lua
shutdown [-r] [-s] <time|now> [reason]
```

## 用途

默认把时间参数解释为分钟，使用 `-s` 时则按秒解释。命令会先输出状态信息，等待指定延迟，随后清空终端、删除 `/tmp/.root`，最后执行关机或重启。

## 参数

- `time|now`: 关机前延迟。`now` 表示零延迟；否则默认按分钟解释，除非使用了 `-s`。
- `reason`: 可选文本，会附加到延迟开始前显示的状态消息中。

## 选项

- `-r`: 执行重启，而不是关机。
- `-s`: 把数值时间参数按秒解释，而不是按分钟。

## 示例

```lua
shutdown now maintenance
```
立即关机，并把 `maintenance` 作为原因显示出来。

```lua
shutdown 5 backup
```
五分钟后关机，并显示原因 `backup`。

```lua
shutdown -r -s 30 deploy
```
三十秒后重启，并显示原因 `deploy`。

## 备注

- 除非使用字面量 `now`，否则必须给出一个数值时间参数。
- 命令会在真正关机或重启前删除 `/tmp/.root`，因此临时提权状态不会跨重启保留。
