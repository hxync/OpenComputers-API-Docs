# ps

## 概述

显示当前进程树以及基础运行时统计信息。

## 可用性

- 仓库：`opencomputers`
- 系统：`plan9k`
- 类型：`command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\ps.lua`

## 语法

```lua
ps
```

## 用途

检查内存中的进程表，并为每个进程或线程根输出一行。输出内容包括 PID、事件处理器数量、线程数量、句柄数量以及记录的命令名。

## 示例

```lua
ps
```

输出当前进程树。

## 备注

- 这个实现会读取非公开的进程内部结构，因此它的字段布局可能随着内核变动而变化。
- `CMD` 列会用树状缩进标出子命令，从而直观看到父子关系。
