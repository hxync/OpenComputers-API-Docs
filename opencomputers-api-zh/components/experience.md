# experience

## 概述

`experience` 组件属于经验升级。经验值实际保存在升级物品本身中，这个 Lua 接口主要用来读取当前储存等级，以及手动把可消耗的经验来源转换进升级内部。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`experience`
- 常见宿主：安装了经验升级的机器人和无人机

## 作用

- 最多在升级内部存储 `30` 级经验。
- 随着经验等级提升，扩展升级自身的内部能量缓冲。
- 当升级物品被转移到另一台设备时，已经存储的经验也会一并带过去。
- 通过 `consume()` 手动消耗经验瓶或附魔物品，把经验转存到升级内。

这个升级还可以通过 OpenComputers 其他机制在游戏行为中获得经验；本组件只负责暴露当前存量和手动消耗物品的接口。

## API

### level

- 语法：`experience.level()`
- 返回值：`number`
- 用途：读取当前存储的经验等级，返回带小数的等级值。

整数部分表示当前整级，小数部分表示到下一级的进度。

### consume

- 语法：`experience.consume()`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：消耗当前选中物品槽中的一个物品，并把其中的经验值转换为升级内部经验。

可接受的物品：

- `minecraft:experience_bottle`
- 任意可以提取出正附魔价值的附魔物品

失败原因：

- `max level`
- `no item`
- `could not extract experience from item`
- `could not consume item`

## 使用说明

- 该升级最多只能保存 `30` 级经验。
- 经验瓶提供的经验值是随机的，和原版经验瓶的实现一致。
- 附魔物品会根据自身附魔计算经验价值；没有可提取附魔价值的普通物品会被拒绝。
- `consume()` 永远操作宿主当前选中的物品槽，因此脚本如果需要指定物品，必须先切换选中槽位。

## 示例

```lua
local component = require("component")
local robot = require("robot")
local xp = component.experience

robot.select(1)

local ok, reason = xp.consume()
if ok then
  print(("Stored level: %.2f"):format(xp.level()))
else
  print("Consume failed:", reason)
end
```

## 相关内容

- `robot`
- `drone`
- `generator`
