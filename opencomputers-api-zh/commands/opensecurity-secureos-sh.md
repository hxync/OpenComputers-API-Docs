# sh

## 概述

启动 SecureOS 标准交互式 shell。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\sh.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\sh`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\sh`

## 语法

```lua
sh
```

## 用途

这是 SecureOS 自带的标准 shell。它支持带引号参数、环境变量展开、使用 `*` 和 `?` 的基础通配展开、基础重定向与管道，以及通过 `alias`、`unalias` 或 `shell` API 管理别名。

## 示例

```lua
sh
```
打开一个新的交互式 SecureOS shell 会话。

```lua
echo 'a b'
```
输出一个带空格且不会触发变量展开的字面字符串。

```lua
ls | cat > files.txt
```
在 SecureOS shell 中使用基础的管道和重定向链。

## 备注

- 和 OpenOS 的手册描述相比，SecureOS 内置说明会明确把它的通配、重定向和管道能力称为基础支持。
