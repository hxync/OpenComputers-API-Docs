# storagedrawers

## 概述

本页说明 OpenComputers 对 Storage Drawers 的集成接口。实现了抽屉组接口的方块会作为 Lua 组件暴露，可用于读取抽屉数量、库存占用和存储物品信息。

## 可用性

- 依赖: `storagedrawers`
- 标签: `integration-required`

## 组件名

- `drawer`

## 主要用途

- 监控整面抽屉墙的库存水平。
- 在抽屉数量低于阈值时触发补货或自动合成。
- 在向抽屉导入物品前判断对应槽位是否为空。

## 组件

### drawer

`drawer` 组件对应实现了 Storage Drawers `IDrawerGroup` 接口的方块。

#### `getDrawerCount()`

- 语法: `getDrawerCount(): number`
- 用途: 返回当前方块暴露出来的抽屉槽总数。
- 返回值:
  - 抽屉槽数量。

#### `getItemCount(drawerSlot)`

- 语法: `getItemCount(drawerSlot: number): number`
- 用途: 返回指定抽屉槽当前存放的物品数量。
- 参数:
  - `drawerSlot: number`
    抽屉槽位编号，Lua 侧从 `1` 开始计数。
- 返回值:
  - 该槽位中的当前物品数量。
- 说明:
  - Lua 侧的一基索引会在驱动内部换算成 Java 的零基索引。
  - 访问不存在或被禁用的抽屉槽位会抛出 `no drawer found at slot N`。

#### `getMaxCapacity(drawerSlot)`

- 语法: `getMaxCapacity(drawerSlot: number): number`
- 用途: 返回指定抽屉槽理论上最多能容纳多少物品。
- 参数:
  - `drawerSlot: number`
    抽屉槽位编号，Lua 侧从 `1` 开始计数。
- 返回值:
  - 该槽位的最大存储容量。
- 说明:
  - 非法槽位同样会抛出 `no drawer found at slot N`。

#### `getItemName(drawerSlot)`

- 语法: `getItemName(drawerSlot: number): string | nil, string?`
- 用途: 返回指定抽屉槽当前物品的未本地化名称。
- 参数:
  - `drawerSlot: number`
    抽屉槽位编号，Lua 侧从 `1` 开始计数。
- 返回值:
  - 非空时返回未本地化名称，例如 `item.ingotIron`。
  - 空抽屉时返回 `nil` 和错误提示。
- 说明:
  - 这个接口返回的不是玩家界面里看到的本地化名称，而是源码使用的未本地化标识。
  - 空抽屉不会抛错，而是返回 `nil` 加消息。

#### `getItemDamage(drawerSlot)`

- 语法: `getItemDamage(drawerSlot: number): number | nil, string?`
- 用途: 返回指定抽屉槽当前物品的 damage 值或元数据值。
- 参数:
  - `drawerSlot: number`
    抽屉槽位编号，Lua 侧从 `1` 开始计数。
- 返回值:
  - 非空时返回数值 damage 或 metadata。
  - 空抽屉时返回 `nil` 和错误提示。
- 说明:
  - 这个值适合用来区分共用同一物品 ID 的不同子类型。
  - 某些极短暂的过渡状态下，非空抽屉也可能出现数量读取异常；如果要做稳健判断，最好把 `getItemName` 与 `getItemCount` 结合起来看。

#### 示例

```lua
local component = require("component")
local drawer = component.drawer

for slot = 1, drawer.getDrawerCount() do
  local name, message = drawer.getItemName(slot)
  local count = drawer.getItemCount(slot)
  local capacity = drawer.getMaxCapacity(slot)

  if name then
    print(slot, name, count .. "/" .. capacity)
  else
    print(slot, "empty", message)
  end
end
```

## 相关内容

- `betterstorage`
- `appeng`
