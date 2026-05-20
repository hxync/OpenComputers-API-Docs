# man

## 概述

使用当前分页器打开内置手册页。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\man.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\man`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\man`

## 语法

```lua
man TOPIC
```

## 用途

在 `MANPATH` 列出的每个目录中查找指定主题，用 `shell.resolve` 解析目标条目，并通过 `PAGER` 环境变量指定的分页器打开第一个匹配到的手册文件。

## 参数

- `TOPIC`: 要在手册路径中查找的程序、库或主题名称。

## 示例

```lua
man ls
```
打开 `ls` 命令的手册页。

```lua
man shell
```
如果 `MANPATH` 中存在对应条目，则显示 `shell` 库的文档。

## 备注

- 命令依赖 shell 环境中正确配置的 `MANPATH` 和 `PAGER`。
- 如果没有找到匹配条目，程序会输出 `No manual entry for <topic>` 并以错误状态退出。
