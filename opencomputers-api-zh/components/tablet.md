# tablet

## 概述

本页说明真实 `tablet` 组件上的接口。它主要暴露“当前手持平板玩家的朝向信息”，适合给那些依赖玩家朝向的平板升级使用。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`tablet`
- 对应实现：`li.cil.oc.server.component.Tablet`
- 常见宿主：平板电脑自身

## 作用

- 读取当前持有平板玩家的俯仰角。
- 读取当前持有平板玩家的水平朝向角。

这些值在平板安装了依赖玩家朝向的升级时尤其有用。

## 与平板有关的重要行为

- 平板电脑是便携式电脑，但不像已放置电脑那样在玩家下线重登或跨维度后继续保持完整持久运行状态。
- T2 平板在“潜行右键”时会打开备用容器界面，并且会强制关闭平板。
- 对着方块持续按住右键大约半秒后，会触发平板分析流程；已安装升级可以向最终产生的 `tablet_use` 信号附加分析数据。

这些内容不是 `tablet` 组件自己的回调，但在设计平板程序时很重要。

## API

### getPitch

- 语法：`tablet.getPitch()`
- 返回：`number`
- 用途：返回当前手持平板玩家的俯仰角。

解释方式：

- 负值表示玩家向上看。
- 正值表示玩家向下看。

### getYaw

- 语法：`tablet.getYaw()`
- 返回：`number`
- 用途：返回当前手持平板玩家的水平朝向角。

解释方式：

- 这是 Minecraft 常见的水平方向旋转角度值。

## 示例

```lua
local component = require("component")
local tablet = component.tablet

print("Yaw:", tablet.getYaw())
print("Pitch:", tablet.getPitch())
```

## 相关内容

- `opencomputers-upgrade-navigation`
- `piston`
- `sign`
