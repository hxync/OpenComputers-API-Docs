# sh

## 概述

启动 Plan9k 标准交互式 shell。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sh.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\sh`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\sh`

## 语法

```lua
sh
```

## 用途

Plan9k 自带一个交互式 shell，沿用内置 `sh` 手册描述的基本命令语言：支持带引号参数、环境变量展开、通配展开、重定向、管道和别名。它也是 Plan9k 终端会话默认面向用户的命令解释器。

## 示例

```lua
sh
```
打开一个新的交互式 Plan9k shell 会话。

```lua
cp /bin/* /usr/bin
```
使用 shell 的通配展开，在一条命令里复制多个文件。

```lua
cat < input.txt >> output.txt
```
通过重定向从 `input.txt` 读取，并把结果追加到 `output.txt`。

## 备注

- 这个 shell 使用的内置文档与 OpenOS 的 `sh` 手册共用，因此描述出的功能集基本一致。
