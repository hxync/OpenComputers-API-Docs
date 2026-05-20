# robot

## 概述

本页说明真实 `robot` 组件上的“机器人专属接口”。这些接口只记录机器人独有的控制能力；机器人还会继承许多共享接口，例如代理交互、物品栏、储罐、红石和各种升级接口，那些内容分别在其他页面中单独记录。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`robot`
- 对应实现：`li.cil.oc.server.component.Robot`
- 常见宿主：机器人自身

## 本页覆盖内容

- 机器人移动
- 机器人转向
- 当前工具耐久读取
- 活动灯颜色控制

## 本页不重复展开的内容

机器人同时还继承了大量其他接口组，例如：

- `swing`、`use`、`place` 等共享代理交互接口
- 内部物品栏控制
- 通过升级获得的外部物品栏交互
- 储罐相关接口
- 红石相关接口

为了避免重复，这些内容分别在对应页面中记录。

## 方向规则

移动方向使用 OpenComputers 常见的“相对机器人朝向”的方向常量。

常见写法：

- `sides.front`
- `sides.back`
- `sides.up`
- `sides.down`

左右方向会随机器人当前朝向变化。

## API

### getLightColor

- 语法：`robot.getLightColor()`
- 返回：`number`
- 用途：返回当前活动灯颜色，格式为 `0xRRGGBB`。

### setLightColor

- 语法：`robot.setLightColor(value)`
- 参数：
  - `value:number`：`0xRRGGBB` 形式的 RGB 颜色值。
- 返回：`number`
- 用途：设置活动灯颜色，并返回最终生效的颜色值。

行为细节：

- 这个调用会让当前计算机短暂停顿。

### durability

- 语法：`robot.durability()`
- 返回：
  - 成功时：`number`
  - 失败时：`nil, "no tool equipped"` 或 `nil, "tool cannot be damaged"`
- 用途：读取当前已装备工具的耐久状态。

返回值说明：

- 具体数值含义取决于该工具对应的耐久提供器实现。
- 它更适合作为脚本读取的统一耐久表示，而不一定等同于原版物品 damage 值。

### move

- 语法：`robot.move(direction)`
- 参数：
  - `direction:number`：相对于机器人自身的移动方向。
- 返回：
  - 成功时：`true`
  - 失败时：`nil, reason`
- 用途：尝试让机器人朝指定方向移动一格。

可能的失败原因包括：

- `already moving`
- `not enough energy`
- `impossible move`
- 由碰撞或目标内容检查返回的其他阻挡原因

行为细节：

- 移动会消耗机器人能量。
- 如果先预扣了能量但最终移动失败，能量会退回。
- 因碰撞类失败时会产生短暂停顿和粒子效果。

### turn

- 语法：`robot.turn(clockwise)`
- 参数：
  - `clockwise:boolean`：`true` 表示右转，`false` 表示左转。
- 返回：
  - 成功时：`true`
  - 失败时：`nil, "not enough energy"`
- 用途：让机器人原地旋转。

行为细节：

- 转向会消耗机器人能量。
- 当前计算机会暂停到配置好的转向延迟结束。

## 示例

```lua
local robot = require("robot")
local sides = require("sides")

print(string.format("灯光颜色: 0x%06X", robot.getLightColor()))
robot.setLightColor(0x33CCFF)

local durability, err = robot.durability()
print("耐久状态：", durability, err)

local ok, why = robot.move(sides.front)
print("移动结果：", ok, why)

if ok then
  robot.turn(true)
end
```

## 相关内容

- `opencomputers-agent`
- `opencomputers-inventory-control`
- `opencomputers-redstone-vanilla`
