# tractor_beam

## 概述

`tractor_beam` 组件是牵引光束升级暴露出的拾取接口。它允许支持该升级的宿主在不亲自移动到物品上的情况下收集附近掉落物。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`tractor_beam`
- 常见宿主：安装了牵引光束升级的机器人、无人机，以及由玩家携带的相关宿主

## 作用

- 在宿主周围 `3` 格半径范围内搜索掉落物实体。
- 从符合条件的掉落物堆中随机挑选一个。
- 尝试把这个掉落物堆收集进宿主库存或玩家库存。

## API

### suck

- 语法：`tractor_beam.suck()`
- 返回值：`boolean`
- 用途：尝试收集附近的一个掉落物堆。

返回值含义：

- `true`：附近至少有一个掉落物堆被成功收集了部分或全部物品。
- `false`：附近没有可收集的掉落物，或者没有成功收进库存。

## 使用说明

- 只有拾取延迟已经结束的掉落物才会被纳入搜索。
- 这个升级只处理世界中的掉落实体，不会作用于方块或容器。
- 成功拾取时会按 OpenComputers 常规 `suck` 延迟暂停调用上下文。
- 对无人机类宿主来说，插入库存时会优先从当前选中槽位开始，再循环到后续槽位。

## 示例

```lua
local component = require("component")
local beam = component.tractor_beam

if beam.suck() then
  print("Picked up an item.")
else
  print("Nothing collectable in range.")
end
```

## 相关内容

- `robot`
- `drone`
- `opencomputers-inventory-world-control`
