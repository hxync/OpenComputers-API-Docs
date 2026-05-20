# cat

## 概述

把文件内容或标准输入原样输出到标准输出。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\cat.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\cat`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\cat`

## 语法

```lua
cat [FILE...]
```

## 用途

把一个或多个文件顺序输出到标准输出。如果没有提供参数，或者参数中包含 `-`，命令就会改为从标准输入读取内容。

## 参数

- `FILE...`: 按顺序输出的文件。使用 `-` 表示在该位置从标准输入读取。

## 示例

```lua
cat
```
把标准输入原样回显到标准输出，直到输入流结束。

```lua
cat /etc/motd
```
输出 `/etc/motd` 的内容。

```lua
cat a.txt b.txt
```
先输出 `a.txt`，再紧接着输出 `b.txt` 的内容。

## 备注

- 目录参数会被直接拒绝，命令不会递归读取目录内容。
- 只要有任意一个请求的文件无法打开，命令就会立刻停止，并以错误状态退出。
