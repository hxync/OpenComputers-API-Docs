# sandbox

## 概述

用进程拉起、控制组和标准流重定向拼装一个小型 Plan9k 沙箱执行流水线。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sandbox.lua`

## 语法

```lua
sandbox [SUBCOMMAND...]
```
```lua
sandbox module spawn FILE [args N ARG...] join
```

## 用途

按从左到右的顺序解释一串沙箱子命令。你可以创建模块和组件命名空间，对白名单或黑名单中的组件做访问限制，把标准输入、输出、错误重定向到 `/dev/null`，拉起一个或多个进程，并可选地等待最后一个进程结束。

## 参数

- `spawn FILE`: 从给定脚本或可执行路径启动一个进程。
- `args N ARG...`: 紧跟在 `spawn FILE` 后面时，表示把接下来的 `N` 个值作为进程参数传入。
- `join`: 等待当前沙箱命令行里最后一次拉起的进程结束。
- `module`: 在后续启动进程前创建新的模块命名空间或控制组。
- `component`: 根据当前白名单和黑名单创建组件访问控制组。
- `wl ADDRESS`: 把组件地址加入下一次 `component` 步骤会使用的白名单。
- `bl ADDRESS`: 把组件地址加入下一次 `component` 步骤会使用的黑名单。
- `quietin`: 把后续进程的标准输入替换成 `/dev/null`。
- `quietout`: 把后续进程的标准输出替换成 `/dev/null`。
- `quieterr`: 把后续进程的标准错误替换成 `/dev/null`。

## 选项

- `-h, --help`: 输出内置帮助文本和示例。

## 示例

```lua
sandbox module spawn /bin/a.lua join
```
在新的模块命名空间中运行 `/bin/a.lua`，并等待它结束。

```lua
sandbox wl 0ab component spawn /bin/a.lua
```
创建组件访问控制组后启动 `/bin/a.lua`，只显式允许白名单中的组件。

```lua
sandbox quietout quieterr spawn /bin/a.lua
```
启动 `/bin/a.lua`，并把标准输出和标准错误都静音。

## 备注

- 这个解析器是顺序且带状态的：`wl` 和 `bl` 会不断累计地址，直到后面的 `component` 步骤读取并使用它们。
- `join` 只能等待当前命令行里最后一次启动的那个进程。
