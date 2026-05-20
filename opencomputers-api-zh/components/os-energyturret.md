# os_energyturret

## 概述

`os_energyturret` 组件用于控制 OpenSecurity 能量炮塔。它可以读取当前炮塔朝向、移动到新的偏航和俯仰角、伸缩炮管支架、进行武装或解除武装、切换供电状态，并发射能量弹。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_energyturret`
- 常见宿主：放置在世界中的 OpenSecurity 能量炮塔

## 用法

```lua
local component = require("component")
local turret = component.os_energyturret

if not turret then
  error("未安装 os_energyturret 组件")
end
```

## 朝向与运动

- `yaw` 以角度保存，并会约束到 `0` 到小于 `360` 的范围。
- `pitch` 以角度保存，并会限制在 `-90` 到 `90` 之间。
- 实际可达到的俯仰角还会受到当前支架长度和炮塔正放/倒放状态的进一步限制。
- `moveToRadians` 会先把弧度转换成角度，再写入内部目标值。

基础转动速度：

- 每 tick `4` 度

移动升级：

- `2` 号槽位：每 tick 额外 `+2.5` 度
- `3` 号槽位：每 tick 额外 `+2.5` 度

## 支架规则

- 请求的支架长度会被限制在 `0` 到 `2` 之间。
- 如果炮塔上方或下方没有额外空气方块，长度超过 `0.5` 的延伸将不被允许。
- `extendShaft` 返回的是最终被接受的实际目标长度。

## 供电、冷却与升级

处于供电状态时的被动耗能：

- 每 tick 消耗 `10` 点缓冲能量

伤害升级：

- `0` 号槽位：伤害 `* 3`
- `1` 号槽位：伤害 `* 3`

节能升级：

- `6` 号槽位：发射能耗 `* 0.7`
- `7` 号槽位：发射能耗 `* 0.7`

冷却升级：

- `4` 号槽位：每 tick 额外减少 3 次冷却计数
- `5` 号槽位：每 tick 额外减少 3 次冷却计数

基础射击参数：

- 基础伤害：`3`
- 基础发射能耗：`75`
- 每次开火后的基础冷却：`200` tick

## 接口

### getYaw

- 语法：`turret.getYaw()`
- 返回：`number`
- 用途：返回当前真实偏航角，单位为度。

### getPitch

- 语法：`turret.getPitch()`
- 返回：`number`
- 用途：返回当前真实俯仰角，单位为度。

### isOnTarget

- 语法：`turret.isOnTarget()`
- 返回：`boolean, delta`
- 用途：报告炮塔是否已经到达当前目标朝向。

行为说明：

- `delta` 是当前偏航误差和俯仰误差的绝对值之和。
- 当 `delta < 0.5` 时，布尔值会变为 `true`。

### isReady

- 语法：`turret.isReady()`
- 返回：`boolean`
- 用途：判断炮塔是否已经武装、冷却完成并且炮管完全就绪，能够发射。

### isPowered

- 语法：`turret.isPowered()`
- 返回：`boolean`
- 用途：返回炮塔当前是否处于供电状态。

### extendShaft

- 语法：`turret.extendShaft(length)`
- 返回：`number`
- 用途：设置炮管支架的目标伸出长度。

参数说明：

- `length:number`：请求的支架长度，范围 `0` 到 `2`。

示例：

```lua
print("最终接受的支架长度：", turret.extendShaft(1.25))
```

### getShaftLength

- 语法：`turret.getShaftLength()`
- 返回：`number`
- 用途：返回当前真实支架长度。

### moveTo

- 语法：`turret.moveTo(yaw, pitch)`
- 返回：`true`
- 用途：以角度形式设置炮塔目标朝向。

参数说明：

- `yaw:number`：目标偏航角，单位度
- `pitch:number`：目标俯仰角，单位度

行为说明：

- 如果炮塔未供电，会抛出 `powered off`。

示例：

```lua
assert(turret.moveTo(90, 15))
```

### moveToRadians

- 语法：`turret.moveToRadians(yaw, pitch)`
- 返回：`true`
- 用途：以弧度形式设置炮塔目标朝向。

参数说明：

- `yaw:number`：目标偏航角，单位弧度
- `pitch:number`：目标俯仰角，单位弧度

行为说明：

- 如果炮塔未供电，会抛出 `powered off`。

示例：

```lua
assert(turret.moveToRadians(math.pi / 2, 0))
```

### setArmed

- 语法：`turret.setArmed(armed)`
- 返回：`true`
- 用途：设置炮塔是否武装。

参数说明：

- `armed:boolean`：目标武装状态。

行为说明：

- 解除武装后，炮管会随时间缩回，`isReady()` 也会变成 `false`。

### powerOn

- 语法：`turret.powerOn()`
- 返回：`true`
- 用途：恢复炮塔供电状态。

### powerOff

- 语法：`turret.powerOff()`
- 返回：`true`
- 用途：关闭炮塔，并把当前朝向冻结为新的目标位置。

### fire

- 语法：`turret.fire()`
- 返回：`true`
- 用途：发射一枚能量弹。

行为说明：

- 需要炮塔处于供电状态。
- 需要炮塔已经武装。
- 需要炮管完全就绪。
- 开火后会把冷却值设置为 `200` tick，之后再由冷却升级加速下降。
- 会在世界中生成一个 `EntityEnergyBolt` 实体。

常见失败原因：

- `powered off`
- `Not armed`
- `gun hasn't cooled`
- `not enough energy`

示例：

```lua
if turret.isReady() then
  assert(turret.fire())
end
```

## 示例

```lua
turret.powerOn()
turret.setArmed(true)
turret.extendShaft(1.5)
turret.moveTo(180, 10)

repeat
  os.sleep(0.1)
until turret.isOnTarget()

if turret.isReady() then
  turret.fire()
end
```

## 说明

- 炮塔在供电状态下会持续每 tick 耗能；如果连接器无法支付这部分耗能，它会自动掉电。
- 最终发射时使用的实际偏航方向还可能受到 `turretReverseRotation` 配置影响。

## 相关内容

- `component`
- `os_alarm`
