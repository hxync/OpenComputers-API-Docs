# rc

## 概述

查看、调用、启用或禁用 `/etc/rc.d/` 中的服务命令。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\rc.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\rc`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\rc`

## 语法

```lua
rc SERVICE COMMAND [ARGS...]
```
```lua
rc SERVICE
```

## 用途

把命令分派给 `/etc/rc.d/` 下的服务脚本执行。如果只提供服务名，命令会列出该服务导出的可调用函数。

## 参数

- `SERVICE`: 位于 `/etc/rc.d/` 下的服务脚本名，不带 `.lua` 后缀。
- `COMMAND`: 服务命令，例如 `start`、`stop`、`restart`、`enable` 或 `disable`。
- `ARGS...`: 额外参数，会原样传给选定的服务命令。

## 示例

```lua
rc example
```
列出 `example` 服务导出的命令。

```lua
rc example start
```
启动 `example` 服务。

```lua
rc example enable
```
启用 `example`，让它在开机时自动启动。

## 备注

- 内置的 `enable` 和 `disable` 只会更新 `/etc/rc.cfg`，不会立刻启动或停止服务。
- 如果服务脚本自己定义了同名命令函数，它可以覆盖库提供的默认行为。
