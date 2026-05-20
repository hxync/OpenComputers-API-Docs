# bloodmagic

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Blood Magic 集成接口。可用组件覆盖血之祭坛和主仪式石。

## 可用性

- 依赖: `bloodmagic`
- 标签: `integration-required`

## 组件名

- `blood_altar`
- `master_ritual_stone`

## 主要用途

- 监控血之祭坛容量、当前血量和祭坛等级。
- 读取祭坛进度以及各种献祭倍率。
- 检查主仪式石的所有者、当前仪式、冷却和运行时间。

## 组件

### blood_altar

`blood_altar` 用于实现 `IBloodAltar` 的 Blood Magic 血之祭坛。

#### `getCapacity()`

- 语法: `getCapacity(): number`
- 用途: 返回祭坛的总血量容量。
- 返回值:
  - 祭坛能够容纳的最大血量。

#### `getCurrentBlood()`

- 语法: `getCurrentBlood(): number`
- 用途: 返回祭坛当前存储的血量。
- 返回值:
  - 当前血量。

#### `getTier()`

- 语法: `getTier(): number`
- 用途: 返回祭坛当前等级。
- 返回值:
  - 等级编号。

#### `getProgress()`

- 语法: `getProgress(): number`
- 用途: 返回祭坛当前操作进度。
- 返回值:
  - 当前进度值。
- 说明:
  - 这个值就是 `IBloodAltar.getProgress()` 的原始返回值，驱动没有做归一化、百分比换算或范围裁剪。
  - 它通常更适合拿来和同一祭坛前后两次读取结果做比较，而不是当成固定 `0..100` 百分比使用。

#### `getSacrificeMultiplier()`

- 语法: `getSacrificeMultiplier(): number`
- 用途: 返回祭坛的普通献祭倍率。
- 返回值:
  - 当前普通献祭倍率。

#### `getSelfSacrificeMultiplier()`

- 语法: `getSelfSacrificeMultiplier(): number`
- 用途: 返回祭坛的自我献祭倍率。
- 返回值:
  - 当前自我献祭倍率。

#### `getOrbMultiplier()`

- 语法: `getOrbMultiplier(): number`
- 用途: 返回祭坛的血球倍率。
- 返回值:
  - 当前血球倍率。

#### `getDislocationMultiplier()`

- 语法: `getDislocationMultiplier(): number`
- 用途: 返回祭坛的位移倍率。
- 返回值:
  - 当前位移倍率。

#### `getBufferCapacity()`

- 语法: `getBufferCapacity(): number`
- 用途: 返回祭坛的 IO 缓冲容量。
- 返回值:
  - IO 缓冲容量值。

### master_ritual_stone

`master_ritual_stone` 用于实现 `IMasterRitualStone` 的 Blood Magic 主仪式石。

#### `getOwner()`

- 语法: `getOwner(): string`
- 用途: 返回拥有这块主仪式石的玩家名称。
- 返回值:
  - 所有者名称字符串。

#### `getCurrentRitual()`

- 语法: `getCurrentRitual(): string`
- 用途: 返回这块主仪式石当前关联的仪式。
- 返回值:
  - 在正常 Blood Magic 主仪式石实现上返回仪式标识或名称。
  - 如果底层方块没有暴露期望的主石实现，则返回 `"internal error"`。

#### `getCooldown()`

- 语法: `getCooldown(): number`
- 用途: 返回剩余仪式冷却时间。
- 返回值:
  - 冷却值。

#### `getRunningTime()`

- 语法: `getRunningTime(): number`
- 用途: 返回仪式已经运行了多长时间。
- 返回值:
  - 运行时间值。

#### `areTanksEmpty()`

- 语法: `areTanksEmpty(): boolean`
- 用途: 返回这块主仪式石的储罐是否为空。
- 返回值:
  - `true` 表示储罐为空。
  - `false` 表示不为空。

## 示例

```lua
local component = require("component")

if component.isAvailable("blood_altar") then
  local altar = component.blood_altar
  print("Blood:", altar.getCurrentBlood(), "/", altar.getCapacity())
  print("Tier:", altar.getTier())
end

if component.isAvailable("master_ritual_stone") then
  local stone = component.master_ritual_stone
  print("Owner:", stone.getOwner())
  print("Ritual:", stone.getCurrentRitual())
end
```

## 相关内容

- `thaumcraft`
- `vanilla`
