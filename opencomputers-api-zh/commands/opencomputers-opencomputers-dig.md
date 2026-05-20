# dig

## 概述

让机器人持续向下开挖一个方形竖井，直到遇到无法破坏的方块。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\dig\usr\bin\dig.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\dig\usr\man\dig.man`

## 语法

```lua
dig SIZE
```

## 用途

按分层蛇形路线开挖指定边长的方形区域。当机器人背包被装满时，它会返回起点，转向最初位于自己身后的方向，把物品全部丢出，然后继续开挖。

## 参数

- `SIZE`: 要开挖的方形区域边长。

## 选项

- `-s`: 在开挖结束后让机器人关机。

## 示例

```lua
dig 3
```
向下开挖一个 `3 x 3` 的竖井，直到机器人无法继续前进。

## 备注

- 这个实现不会主动做电量预算或中途充电管理。
- 丢弃物会朝向命令启动时机器人身后的那一侧，因此在那里放一个箱子即可自动收集开采物。
