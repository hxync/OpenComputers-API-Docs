# refuel

## 概述

查看、向机器人发电机升级插入燃料，或从中弹出燃料。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\generator\usr\bin\refuel.lua`

## 语法

```lua
refuel
```
```lua
refuel SLOT [AMOUNT]
```
```lua
refuel all
```

## 用途

操作已安装的机器人发电机升级。不带参数时会显示当前发电机中的物品数量；给出数字槽位时会从该槽位补充燃料；给出 `all` 时会依次尝试所有背包槽。

## 参数

- `SLOT`: 在插入或弹出物品前要选中的背包槽位号。
- `AMOUNT`: 可选，物品数量。正数表示向发电机插入燃料，负数表示把对应数量的物品弹回该槽位，省略时默认为最多 `64`。

## 示例

```lua
refuel
```
显示当前发电机里存放了多少物品。

```lua
refuel 3
```
选中 `3` 号槽，并向发电机插入该槽中的最多 64 个物品。

```lua
refuel 2 -10
```
把发电机中的十个物品弹回 `2` 号槽。

```lua
refuel all
```
遍历机器人全部 16 个背包槽，并尝试从每个槽位向发电机插入燃料。

## 备注

- 这个命令故意没有使用 `shell.parse`，因此像 `-10` 这样的负数会被当作位置参数，而不是选项。
- 必须先安装发电机升级，否则命令只会提示缺少该升级并且不会执行实际操作。
