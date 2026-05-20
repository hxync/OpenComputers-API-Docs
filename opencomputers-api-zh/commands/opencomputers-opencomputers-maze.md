# maze

## 概述

使用深度优先搜索生成随机迷宫，并让机器人把它挖出来。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\maze\usr\bin\maze.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\maze\usr\man\maze.man`

## 语法

```lua
maze WIDTH LENGTH
```
```lua
maze WIDTH LENGTH HEIGHT
```
```lua
maze WIDTH LENGTH HEIGHT DIAMETER
```
```lua
maze WIDTH LENGTH HEIGHT DIAMETER REFUEL
```

## 用途

先在内存中建立访问网格，再用深度优先回溯算法生成路径，并把迷宫实体挖掘到世界中。宽度和长度都必须至少为 `2`；高度默认是 `1`，单元直径默认是 `1`，自动补燃料默认关闭。

## 参数

- `WIDTH`: 迷宫的单元宽度。
- `LENGTH`: 迷宫的单元长度。
- `HEIGHT`: 每个迷宫单元的垂直高度，单位是方块。
- `DIAMETER`: 每个迷宫单元的水平尺寸，单位是方块。
- `REFUEL`: 字面量 `true` 或 `false`，表示是否使用发电机升级自动补燃料。

## 示例

```lua
maze 10 10
```
使用默认高度、直径和补燃料设置，挖出一个 `10 x 10` 迷宫。

```lua
maze 10 10 2 3 true
```
挖出一个 `10 x 10` 的迷宫，单元高度为 `2`、单元直径为 `3`，并启用自动补燃料。

## 备注

- 如果把 `REFUEL` 设为 `true`，机器人必须安装发电机升级。
- 增大 `DIAMETER` 会扩大迷宫在 Minecraft 世界中的真实占地，而不只是逻辑单元大小。
