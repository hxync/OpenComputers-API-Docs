# robot

## 概述

围绕机器人组件提供的高层便捷封装，覆盖移动、背包、工具和流体罐操作。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\lua\component\robot\lib\robot.lua`

## 用法

```lua
local robot = require("robot")
```

## 接口

### robot.back()

- 说明: 让机器人向后移动一格。

```lua
robot.back()
```

### robot.compare(fuzzy)

- 说明: 比较当前选中物品与机器人前方的方块或容器目标。
- 参数:
  - `fuzzy`

```lua
robot.compare(nil)
```

### robot.compareDown(fuzzy)

- 说明: 比较当前选中物品与机器人下方目标。
- 参数:
  - `fuzzy`

```lua
robot.compareDown(nil)
```

### robot.compareFluid()

- 说明: 比较当前选中流体罐中的流体与前方目标中的流体。

```lua
robot.compareFluid()
```

### robot.compareFluidDown()

- 说明: 比较当前选中流体罐中的流体与机器人下方目标中的流体。

```lua
robot.compareFluidDown()
```

### robot.compareFluidTo(...)

- 说明: 比较当前选中流体罐与另一个机器人流体罐中的流体。
- 参数:
  - `...`

```lua
robot.compareFluidTo(nil)
```

### robot.compareFluidUp()

- 说明: 比较当前选中流体罐中的流体与机器人上方目标中的流体。

```lua
robot.compareFluidUp()
```

### robot.compareTo(...)

- 说明: 比较当前选中槽位与另一个机器人槽位中的物品。
- 参数:
  - `...`

```lua
robot.compareTo(nil)
```

### robot.compareUp(fuzzy)

- 说明: 比较当前选中物品与机器人上方目标。
- 参数:
  - `fuzzy`

```lua
robot.compareUp(nil)
```

### robot.count(...)

- 说明: 返回某个槽位中当前存放的物品数量。
- 参数:
  - `...`

```lua
robot.count(nil)
```

### robot.detect()

- 说明: 检查机器人正前方是否有方块或实体阻挡。

```lua
robot.detect()
```

### robot.detectDown()

- 说明: 检查机器人下方空间是否被占用。

```lua
robot.detectDown()
```

### robot.detectUp()

- 说明: 检查机器人上方空间是否被占用。

```lua
robot.detectUp()
```

### robot.down()

- 说明: 让机器人向下移动一格。

```lua
robot.down()
```

### robot.drain(count)

- 说明: 从前方目标抽取流体到当前选中流体罐中。
- 参数:
  - `count`

```lua
robot.drain(nil)
```

### robot.drainDown(count)

- 说明: 从下方目标抽取流体到当前选中流体罐中。
- 参数:
  - `count`

```lua
robot.drainDown(nil)
```

### robot.drainUp(count)

- 说明: 从上方目标抽取流体到当前选中流体罐中。
- 参数:
  - `count`

```lua
robot.drainUp(nil)
```

### robot.drop(count)

- 说明: 把当前选中槽位中的物品丢到机器人前方。
- 参数:
  - `count`

```lua
robot.drop(nil)
```

### robot.dropDown(count)

- 说明: 把当前选中槽位中的物品向下方丢出或放入容器。
- 参数:
  - `count`

```lua
robot.dropDown(nil)
```

### robot.dropUp(count)

- 说明: 把当前选中槽位中的物品向上方丢出或放入容器。
- 参数:
  - `count`

```lua
robot.dropUp(nil)
```

### robot.durability()

- 说明: 返回机器人已安装工具升级的耐久度信息。

```lua
robot.durability()
```

### robot.fill(count)

- 说明: 把当前选中流体罐中的流体注入前方目标。
- 参数:
  - `count`

```lua
robot.fill(nil)
```

### robot.fillDown(count)

- 说明: 把当前选中流体罐中的流体注入机器人下方目标。
- 参数:
  - `count`

```lua
robot.fillDown(nil)
```

### robot.fillUp(count)

- 说明: 把当前选中流体罐中的流体注入机器人上方目标。
- 参数:
  - `count`

```lua
robot.fillUp(nil)
```

### robot.forward()

- 说明: 让机器人向前移动一格。

```lua
robot.forward()
```

### robot.getLightColor()

- 说明: 返回机器人当前设置的指示灯颜色。

```lua
robot.getLightColor()
```

### robot.inventorySize()

- 说明: 返回机器人当前拥有的背包槽位数量。

```lua
robot.inventorySize()
```

### robot.level()

- 说明: 若安装了经验升级则返回机器人经验等级，否则返回 `0`。

```lua
robot.level()
```

### robot.name()

- 说明: 返回机器人的当前名称。

```lua
robot.name()
```

### robot.place(side, sneaky)

- 说明: 把当前选中物品放置或使用到前方目标上，并可选指定子方位和潜行模式。
- 参数:
  - `side`
  - `sneaky`

```lua
robot.place(nil, nil)
```

### robot.placeDown(side, sneaky)

- 说明: 把当前选中物品放置或使用到机器人下方目标上。
- 参数:
  - `side`
  - `sneaky`

```lua
robot.placeDown(nil, nil)
```

### robot.placeUp(side, sneaky)

- 说明: 把当前选中物品放置或使用到机器人上方目标上。
- 参数:
  - `side`
  - `sneaky`

```lua
robot.placeUp(nil, nil)
```

### robot.select(...)

- 说明: 通过机器人组件获取或切换当前选中的背包槽位。
- 参数:
  - `...`

```lua
robot.select(nil)
```

### robot.selectTank(tank)

- 说明: 获取或切换当前选中的流体罐。
- 参数:
  - `tank`

```lua
robot.selectTank(nil)
```

### robot.setLightColor(value)

- 说明: 设置机器人指示灯颜色，并返回组件调用结果。
- 参数:
  - `value`

```lua
robot.setLightColor(nil)
```

### robot.space(...)

- 说明: 返回某个槽位还能容纳多少物品。
- 参数:
  - `...`

```lua
robot.space(nil)
```

### robot.suck(count)

- 说明: 从前方吸取掉落物或容器物品到当前选中槽位。
- 参数:
  - `count`

```lua
robot.suck(nil)
```

### robot.suckDown(count)

- 说明: 从下方吸取掉落物或容器物品到当前选中槽位。
- 参数:
  - `count`

```lua
robot.suckDown(nil)
```

### robot.suckUp(count)

- 说明: 从上方吸取掉落物或容器物品到当前选中槽位。
- 参数:
  - `count`

```lua
robot.suckUp(nil)
```

### robot.swing(side, sneaky)

- 说明: 使用已安装工具作用于机器人前方的方块或实体。
- 参数:
  - `side`
  - `sneaky`

```lua
robot.swing(nil, nil)
```

### robot.swingDown(side, sneaky)

- 说明: 使用已安装工具作用于机器人下方目标。
- 参数:
  - `side`
  - `sneaky`

```lua
robot.swingDown(nil, nil)
```

### robot.swingUp(side, sneaky)

- 说明: 使用已安装工具作用于机器人上方目标。
- 参数:
  - `side`
  - `sneaky`

```lua
robot.swingUp(nil, nil)
```

### robot.tankCount()

- 说明: 返回机器人当前拥有的流体罐数量。

```lua
robot.tankCount()
```

### robot.tankLevel(...)

- 说明: 返回某个流体罐当前存储的流体量。
- 参数:
  - `...`

```lua
robot.tankLevel(nil)
```

### robot.tankSpace(...)

- 说明: 返回某个流体罐还能容纳多少流体。
- 参数:
  - `...`

```lua
robot.tankSpace(nil)
```

### robot.transferFluidTo(...)

- 说明: 把当前选中流体罐中的流体转移到另一个机器人流体罐。
- 参数:
  - `...`

```lua
robot.transferFluidTo(nil)
```

### robot.transferTo(...)

- 说明: 把当前选中槽位中的物品转移到另一个机器人槽位。
- 参数:
  - `...`

```lua
robot.transferTo(nil)
```

### robot.turnAround()

- 说明: 通过连续两次同向四分之一转身让机器人原地掉头。

```lua
robot.turnAround()
```

### robot.turnLeft()

- 说明: 让机器人向左旋转 90 度。

```lua
robot.turnLeft()
```

### robot.turnRight()

- 说明: 让机器人向右旋转 90 度。

```lua
robot.turnRight()
```

### robot.up()

- 说明: 让机器人向上移动一格。

```lua
robot.up()
```

### robot.use(side, sneaky, duration)

- 说明: 朝前方激活或使用当前选中物品，并可选潜行或持续使用。
- 参数:
  - `side`
  - `sneaky`
  - `duration`

```lua
robot.use(nil, nil, nil)
```

### robot.useDown(side, sneaky, duration)

- 说明: 朝下方激活或使用当前选中物品。
- 参数:
  - `side`
  - `sneaky`
  - `duration`

```lua
robot.useDown(nil, nil, nil)
```

### robot.useUp(side, sneaky, duration)

- 说明: 朝上方激活或使用当前选中物品。
- 参数:
  - `side`
  - `sneaky`
  - `duration`

```lua
robot.useUp(nil, nil, nil)
```

## 备注

- 这些封装会把 `forward`、`swingUp`、`dropDown` 这类方向便捷调用转换成底层组件方法，并自动填入固定的方位常量。
