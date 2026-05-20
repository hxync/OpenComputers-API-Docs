# df

## 概述

报告文件系统容量和剩余空间信息。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\df.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\df`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\df`

## 语法

```lua
df [FILE]...
```

## 用途

显示包含指定路径的文件系统空间信息。如果没有提供路径参数，命令会汇报当前所有已挂载文件系统的容量情况。

## 参数

- `FILE...`: 可选路径，用来指定应当汇报哪些挂载文件系统。

## 示例

```lua
df
```
显示所有已挂载文件系统的全局磁盘占用信息。

```lua
df /home /var
```
显示挂载在 `/home` 和 `/var` 上的文件系统容量信息。
