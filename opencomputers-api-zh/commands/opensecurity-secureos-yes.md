# yes

## 概述

重复输出一段文本，直到进程被中断或输出流关闭。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\yes.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\yes`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\yes`

## 语法

```lua
yes [STRING...]
```
```lua
yes [-V|--version]
```
```lua
yes [-h|--help]
```

## 用途

默认持续输出 `y`，如果给出参数，则把这些词拼成一行并不断重复输出。它通常用来向需要交互式确认的程序持续喂入自动回答。

## 参数

- `STRING...`: 可选，要重复输出的词。省略时程序会重复 `y`。

## 选项

- `-V, --version`: 输出程序版本信息后退出。
- `-h, --help`: 输出内置帮助信息后退出。

## 示例

```lua
yes
```
在每一行输出 `y`，直到进程被停止。

```lua
yes no
```
在每一行输出 `no`。

```lua
yes "I agree" | some-command
```
通过管道持续把 `I agree` 提供给另一个程序。

## 备注

- 可通过 `Ctrl-C`、结束该进程，或关闭下游输出流来停止这个无限循环。
