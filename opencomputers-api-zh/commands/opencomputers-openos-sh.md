# sh

## 概述

启动 OpenOS 标准交互式 shell。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\sh.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\sh`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\sh`

## 语法

```lua
sh
```

## 用途

这是 OpenOS 自带的标准命令解释器。它会把命令行中的第一个词解析为程序名，把剩余词作为参数传给程序，支持 `$NAME` 和 `${NAME}` 形式的环境变量展开、单引号和双引号引用、`*` 与 `?` 的通配展开，并实现重定向、管道和别名功能。

## 示例

```lua
sh
```
打开一个新的交互式 OpenOS shell 会话。

```lua
echo "a b"
```
通过引号包裹带空格的字符串并输出它。

```lua
ls | cat > files.txt
```
把 `ls` 的输出通过 `cat` 管道后重定向写入 `files.txt`。

## 备注

- 单引号会抑制变量展开，而双引号仍然允许变量展开。
- 别名可以通过 `alias` 创建，也可以通过 `unalias` 或 `shell` API 删除。
