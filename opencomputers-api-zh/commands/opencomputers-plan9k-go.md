# go

## 概述

让机器人按指定的正整数次数移动或转向。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\go.lua`

## 语法

```lua
go DIRECTION [COUNT]
```

## 用途

重复执行某一种机器人移动或转向操作。支持的方向有 `forward`、`back`、`up`、`down`、`left` 和 `right`，次数默认是 `1`。

## 参数

- `DIRECTION`: 以下之一：`forward`、`back`、`up`、`down`、`left`、`right`。
- `COUNT`: 可选，正整数重复次数。若给出小数，则会向下取整。

## 示例

```lua
go forward
```
让机器人向前移动一格。

```lua
go left 2
```
让机器人向左转两次。

```lua
go up 3
```
让机器人连续向上移动三次。

## 备注

- 命令会校验次数参数，但不会在单次机器人动作失败时自动提前中止；它只是反复调用所选的机器人 API。
