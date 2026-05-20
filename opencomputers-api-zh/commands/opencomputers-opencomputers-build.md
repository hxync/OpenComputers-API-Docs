# build

## 概述

让机器人按照 `0`/`1` 分层蓝图文件搭建结构。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\builder\usr\bin\build.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\builder\usr\man\build.man`

## 语法

```lua
build PLAN
```
```lua
build PLAN FROM TO
```

## 用途

读取一个蓝图文件，其中 `0` 表示空气，`1` 表示实心方块，而以 `#` 开头的行用于分隔垂直层。机器人会先分析模型、估算所需材料、在存在 `.state` 文件时尝试断点续建，随后搭建指定的切片范围，并在需要时返回箱子补料和充电。

## 参数

- `PLAN`: 要加载的蓝图文件。若未提供后缀，解析器会自动补上 `.plan`。
- `FROM`: 沿蓝图深度轴选择的零基起始切片编号。
- `TO`: 从 `FROM` 开始要搭建的切片数量。省略时会搭建剩余全部切片。

## 示例

```lua
build pyramid.plan
```
分析 `pyramid.plan`，等待确认后搭建完整结构。

```lua
build some.plan 0 2
```
从 `some.plan` 中选择以 `0` 为起点、跨度为 `2` 个深度切片的区段进行搭建。

```lua
build used.plan
```
当存在匹配的 `.state` 文件时，恢复一次先前中断的建造任务。

## 备注

- 机器人默认要求自己前方放有箱子，箱子内装有建筑材料，并且自身背包 `1` 号槽放着一组泥土。
- 普通建筑方块会被吸入 `2` 到 `16` 号槽中使用。
- 如果机器人在远离箱子时电量跌到危险阈值，脚本会先保存状态，然后让机器人关机。
