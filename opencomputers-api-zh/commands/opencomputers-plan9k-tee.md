# tee

## 概述

把标准输入一边追加到文件，一边原样回显到标准输出。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\tee.lua`

## 语法

```lua
tee FILE
```

## 用途

以追加模式打开目标文件，持续从标准输入读取按行组织的数据，把接收到的数据同时写到标准输出，并在每次写入后立刻刷新目标文件。

## 参数

- `FILE`: 要接收追加数据的目标文件。

## 示例

```lua
dmesg | tee /kern.log
```
把内核日志同时显示到屏幕，并追加保存到 `/kern.log`。

## 备注

- 这个实现始终使用追加写入模式，不提供截断覆盖模式。
