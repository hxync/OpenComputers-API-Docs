# opencomputers-agent

## 概述

这个文件名并不对应一个真实独立的 Lua 组件。

本页记录的是“代理类宿主”共享的世界交互接口，主要出现在：

- `robot`
- `drone`

这些接口会通过宿主自己的真实组件暴露出来，例如 `component.robot` 或 `component.drone`，而不是通过一个叫 `opencomputers-agent` 的组件名出现。

## 本页覆盖的宿主

### robot

- 实际 Lua 组件名：`robot`
- 方向参数按机器人朝向解释。
- 世界交互会触发机器人动画，并遵循机器人动作延迟。

### drone

- 实际 Lua 组件名：`drone`
- 方向参数按世界方向直接解释。
- 世界交互暂停时间会比基础动作延迟更长。

## 这些接口的作用

- 读取代理宿主的显示名称。
- 朝某个方向挥动工具或攻击。
- 朝某个方向使用物品或激活方块。
- 把当前选中物品放进世界。

## 方向规则

`swing`、`use`、`place` 的第一个参数都是主要操作方向。

- 对机器人来说，应使用 OpenComputers 常见的相对方向常量。
- 对无人机来说，方向值直接按当前世界方向解释。

可选的 `face` 参数表示要点击目标方块的哪一个面。

行为说明：

- 如果省略 `face`，底层会先尝试主方向，再在需要时尝试其他相邻面。
- `sneaky=true` 表示让这个伪玩家以潜行状态执行操作。

## 返回值约定

`swing` 和 `use` 通常返回“是否成功”以及一个原因字符串。

常见原因字符串包括：

- `entity`
- `block`
- `fire`
- `air`
- `item_interacted`
- `block_activated`
- `item_placed`
- `item_used`

`place` 成功时只返回布尔值；某些失败情况会返回 `nil, reason`。

## API

### name

- 语法：`agent.name()`
- 返回：`string`
- 用途：返回代理宿主当前配置的名称。

对机器人来说，这是机器人显示的标签名；对无人机来说，这是无人机当前名称。

### swing

- 语法：`agent.swing(side[, face[, sneaky]])`
- 参数：
  - `side:number`：主要攻击或左键操作方向。
  - `face:number`：可选，目标方块被点击的那个面。
  - `sneaky:boolean`：可选，是否潜行，默认 `false`。
- 返回：`boolean, string`
- 用途：朝指定方向执行一次“左键式”操作。

行为细节：

- 如果先命中实体，就会用当前装备工具攻击实体。
- 如果命中方块，就会尝试破坏或敲击方块。
- 如果什么都没命中，仍然会额外检查近距离实体和火焰。
- 对矿车会连续补打多次，以提高破坏成功率。

常见返回：

- `true, "entity"`：攻击了实体。
- `true, "block"`：击中了或破坏了方块。
- `true, "fire"`：熄灭了火焰。
- `false, "air"`：没有命中有效目标。

### use

- 语法：`agent.use(side[, face[, sneaky[, duration]]])`
- 参数：
  - `side:number`：主要右键操作方向。
  - `face:number`：可选，点击目标面的方向。
  - `sneaky:boolean`：可选，是否潜行，默认 `false`。
  - `duration:number`：可选，持续使用时长，默认 `0`。
- 返回：
  - 成功时：`boolean, string`
  - 失败时：`false`
- 用途：朝指定方向执行一次“右键式”操作。

行为细节：

- 可以与实体交互。
- 可以激活方块。
- 在宿主规则允许时，可以直接向空气中放置方块。
- 即使没有命中方块或实体，仍会尝试直接使用当前手持物品。

常见成功原因：

- `item_interacted`
- `block_activated`
- `item_placed`
- `item_used`

`duration` 对弓、需蓄力工具等持续使用型物品尤其有意义。

### place

- 语法：`agent.place(side[, face[, sneaky]])`
- 参数：
  - `side:number`：主要放置方向。
  - `face:number`：可选，目标方块上被点击的面。
  - `sneaky:boolean`：可选，是否潜行，默认 `false`。
- 返回：
  - 成功时：`true`
  - 一般失败时：`false`
  - 选中槽为空时：`nil, "nothing selected"`
- 用途：把当前选中槽中的物品尝试放入世界。

行为细节：

- 当前选中槽必须包含可放置的物品堆。
- 如果能够命中目标方块，就按正常的方块放置流程执行。
- 如果没有命中方块，某些宿主在规则允许时仍可直接向空气放置。

## 示例

```lua
local component = require("component")
local sides = require("sides")

local agent = component.robot or component.drone
if not agent then
  error("当前环境没有代理类组件")
end

print("代理名称：", agent.name())

local ok, why = agent.swing(sides.front)
print("挥动结果：", ok, why)

local used, how = agent.use(sides.front, sides.front, false, 0)
print("使用结果：", used, how)
```

## 相关内容

- `robot`
- `drone`
- `opencomputers-inventory-control`
