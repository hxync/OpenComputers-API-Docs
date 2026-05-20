# debug

## 概述

`debug` 组件是 OpenComputers 调试卡暴露出的 Lua 接口。它属于高权限、偏创造模式用途的工具，可用于世界检查、世界修改、命令执行、玩家控制、物品与流体操作，以及调试卡之间的消息通信。

这个组件的权限非常高，足以绕过正常生存流程中的很多限制。绝大多数回调在真正执行前都会先检查调试卡的访问上下文，因此未绑定或无权限的调试卡会先抛出访问错误。

## 可用性

- 仓库：`opencomputers`
- 组件名：`debug`
- 常见宿主：安装了调试卡的电脑或机器人

## 重要说明

- 如果希望命令执行和权限检查使用你自己的权限等级，需要手持调试卡潜行右键，将其绑定到自己。
- `getPlayer` 和 `getWorld` 返回的是包装后的值对象，真正的操作是在这些对象的方法上完成的。
- `sendToDebugCard` 会让目标机器收到一个 `debug_message` 信号。

## 用法

```lua
local component = require("component")
local debug = component.debug

if not debug then
  error("当前环境未安装 debug 调试卡")
end
```

## 调试卡本体接口

### changeBuffer

- 语法：`debug.changeBuffer(value)`
- 返回值：`number`
- 用途：按给定增量修改组件网络的能量缓冲。

参数说明：

- `value:number`：正数表示增加缓冲，负数表示减少缓冲。

示例：

```lua
print("新的缓冲值:", debug.changeBuffer(2500))
```

### getX

- 语法：`debug.getX()`
- 返回值：`number`
- 用途：读取当前宿主容器的 X 坐标。

### getY

- 语法：`debug.getY()`
- 返回值：`number`
- 用途：读取当前宿主容器的 Y 坐标。

### getZ

- 语法：`debug.getZ()`
- 返回值：`number`
- 用途：读取当前宿主容器的 Z 坐标。

示例：

```lua
print("调试卡位置:", debug.getX(), debug.getY(), debug.getZ())
```

### getWorld

- 语法：`debug.getWorld([dimensionId])`
- 返回值：`WorldValue`
- 用途：获取指定维度的世界包装对象；省略参数时返回宿主所在世界。

参数说明：

- `dimensionId:number` 可选：要访问的维度 ID。

如果要先枚举维度，可以先调用 `getWorlds()`。源码直接按维度 ID 取世界对象，所以实际脚本通常应优先访问已经加载的世界。

示例：

```lua
local world = debug.getWorld()
print("维度名:", world.getDimensionName())
```

### getWorlds

- 语法：`debug.getWorlds()`
- 返回值：`table`
- 用途：返回所有已注册的维度 ID，包括已加载和未加载的维度。

示例：

```lua
for _, id in ipairs(debug.getWorlds()) do
  print("维度 ID:", id)
end
```

### getPlayer

- 语法：`debug.getPlayer(name)`
- 返回值：`PlayerValue`
- 用途：按玩家名获取一个玩家包装对象。

参数说明：

- `name:string`：玩家精确名称。

即使目标玩家当前离线，这个包装对象也会先被创建；真正访问玩家时，才会在 `PlayerValue` 的方法中检查在线状态。

示例：

```lua
local steve = debug.getPlayer("Steve")
print(steve.getHealth())
```

### getPlayers

- 语法：`debug.getPlayers()`
- 返回值：`table`
- 用途：列出当前服务器在线玩家名称。

示例：

```lua
for _, name in ipairs(debug.getPlayers()) do
  print("在线玩家:", name)
end
```

### scanContentsAt

- 语法：`debug.scanContentsAt(x, y, z[, worldId])`
- 返回值：`boolean, string, value`
- 用途：检查某个坐标上到底存在什么内容，也可以指定另一个已加载世界。

参数说明：

- `x:number`、`y:number`、`z:number`：绝对方块坐标。
- `worldId:number` 可选：维度 ID。省略时使用宿主当前世界。

返回值说明：

- 第二个返回值表示内容类别，可能是：
  - `"EntityLivingBase"`
  - `"EntityMinecart"`
  - `"air"`
  - `"liquid"`
  - `"replaceable"`
  - `"passable"`
  - `"solid"`
- 第三个返回值是对应的实体对象或方块对象。
- 第一个返回值是状态标志：
  - 空气时为 `false`；
  - 实体、可穿过方块、实体方块时为 `true`；
  - 液体和可替换方块时，它表示源码里模拟假玩家触发破坏事件后，事件是否被取消。

示例：

```lua
local ok, kind, value = debug.scanContentsAt(0, 64, 0)
print("类别:", kind, "标志:", ok)
print("对象:", value)
```

### isModLoaded

- 语法：`debug.isModLoaded(name)`
- 返回值：`boolean`
- 用途：检查某个 Forge 模组或注册 API 是否已存在。

参数说明：

- `name:string`：模组 ID 或 API 名称。

示例：

```lua
if debug.isModLoaded("gregtech") then
  print("GregTech 已加载")
end
```

### runCommand

- 语法：
  - `debug.runCommand(command)`
  - `debug.runCommand(commandsTable)`
- 返回值：`number, string|nil`
- 用途：通过调试卡使用的假玩家命令发送者执行一条或多条命令。

参数说明：

- `command:string`：单条命令。
- `commandsTable:table`：按顺序执行的命令字符串表。

返回值说明：

- 第一个返回值：最后一条命令的数值返回结果。
- 第二个返回值：执行期间收集到的聊天或命令输出文本；如果没有输出则为 `nil`。

示例：

```lua
local count, output = debug.runCommand("/time set day")
print("影响数量:", count)
if output then
  print(output)
end
```

### connectToBlock

- 语法：`debug.connectToBlock(x, y, z)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：把指定坐标处的组件方块接入当前电脑网络。

参数说明：

- `x:number`、`y:number`、`z:number`：目标方块的绝对坐标。

行为说明：

- 如果调试卡之前已经连接过其他远程节点，源码会先断开旧连接，再连接新目标。
- 它既可以连接普通 `Environment` 方块，也能连接带方向节点的 `SidedEnvironment`。
- 源码中的失败原因是 `"no node found at this position"`。

示例：

```lua
local ok, reason = debug.connectToBlock(10, 64, -5)
if not ok then
  io.stderr:write("连接失败: " .. tostring(reason) .. "\n")
end
```

### test

- 语法：`debug.test()`
- 返回值：多组测试用示例值
- 用途：内部测试回调，用于验证 userdata 和一般值类型的转换行为。

它主要用于开发和兼容性验证，不是正常游戏脚本会经常依赖的接口。

### sendToClipboard

- 语法：`debug.sendToClipboard(player, text)`
- 返回值：`boolean[, string]`
- 用途：在目标玩家在线时，向其客户端剪贴板发送文本。

参数说明：

- `player:string`：玩家名。
- `text:string`：要写入剪贴板的文本。

源码中的失败原因是 `"no such player"`。

示例：

```lua
local ok, reason = debug.sendToClipboard("Steve", "坐标已复制")
if not ok then
  print(reason)
end
```

### sendToDebugCard

- 语法：`debug.sendToDebugCard(address, data...)`
- 返回值：没有直接返回值
- 用途：按组件地址向另一张调试卡发送任意负载数据。

参数说明：

- `address:string`：目标调试卡地址。
- `data...`：要附带发送的数据。

目标机器接收到的数据会变成一个 `computer.signal` 事件：

```lua
"debug_message", sourceAddress, port, distance, ...
```

示例：

```lua
debug.sendToDebugCard("01234567-89ab-cdef-0123-456789abcdef", "sync", 42, true)
```

## PlayerValue 接口

`debug.getPlayer(name)` 会返回一个 `PlayerValue`。下面这些方法在玩家离线时都会失败，并返回 `nil, "player is offline"`。

### getWorld

- 语法：`playerValue.getWorld()`
- 返回值：`WorldValue`
- 用途：获取该玩家当前所在世界。

### getGameType

- 语法：`playerValue.getGameType()`
- 返回值：`string`
- 用途：读取玩家当前游戏模式名称，例如 `survival`、`creative`、`adventure`。

### setGameType

- 语法：`playerValue.setGameType(gametype)`
- 返回值：没有直接返回值
- 用途：修改玩家游戏模式。

参数说明：

- `gametype:string`：游戏模式名称。源码中未知名称会回退为 `survival`。

### getPosition

- 语法：`playerValue.getPosition()`
- 返回值：`number, number, number`
- 用途：读取玩家当前位置。

### setPosition

- 语法：`playerValue.setPosition(x, y, z)`
- 返回值：没有直接返回值
- 用途：将玩家直接传送到指定坐标。

### getHealth

- 语法：`playerValue.getHealth()`
- 返回值：`number`
- 用途：读取玩家当前生命值。

### getMaxHealth

- 语法：`playerValue.getMaxHealth()`
- 返回值：`number`
- 用途：读取玩家最大生命值。

### setHealth

- 语法：`playerValue.setHealth(health)`
- 返回值：没有直接返回值
- 用途：直接设置玩家当前生命值。

示例：

```lua
local player = debug.getPlayer("Steve")
local x, y, z = player.getPosition()
print("玩家坐标:", x, y, z)
player.setGameType("creative")
```

## WorldValue 接口

`debug.getWorld(...)` 和 `playerValue.getWorld()` 返回的都是 `WorldValue`。

### 世界状态

#### getDimensionId

- 语法：`world.getDimensionId()`
- 返回值：`number`
- 用途：读取当前维度的数字 ID。

#### getDimensionName

- 语法：`world.getDimensionName()`
- 返回值：`string`
- 用途：读取当前维度名称。

#### getSeed

- 语法：`world.getSeed()`
- 返回值：`number`
- 用途：读取世界种子。

#### isRaining

- 语法：`world.isRaining()`
- 返回值：`boolean`
- 用途：检查当前是否正在下雨。

#### setRaining

- 语法：`world.setRaining(value)`
- 返回值：没有直接返回值
- 用途：开启或关闭下雨状态。

#### isThundering

- 语法：`world.isThundering()`
- 返回值：`boolean`
- 用途：检查当前是否正在打雷。

#### setThundering

- 语法：`world.setThundering(value)`
- 返回值：没有直接返回值
- 用途：开启或关闭雷暴状态。

#### getTime

- 语法：`world.getTime()`
- 返回值：`number`
- 用途：读取当前世界时间。

#### setTime

- 语法：`world.setTime(value)`
- 返回值：没有直接返回值
- 用途：设置当前世界时间。

#### getSpawnPoint

- 语法：`world.getSpawnPoint()`
- 返回值：`number, number, number`
- 用途：读取世界出生点坐标。

#### setSpawnPoint

- 语法：`world.setSpawnPoint(x, y, z)`
- 返回值：没有直接返回值
- 用途：修改世界出生点坐标。

#### playSoundAt

- 语法：`world.playSoundAt(x, y, z, sound, range)`
- 返回值：没有直接返回值
- 用途：在指定坐标播放一个声音效果。

参数说明：

- `sound:string`：内部声音名称。
- `range:number`：源码用于换算可听范围的数值。

### 方块和 Tile 实体检查

#### getBlockId

- 语法：`world.getBlockId(x, y, z)`
- 返回值：`number`
- 用途：读取指定坐标的数字方块 ID。

#### getMetadata

- 语法：`world.getMetadata(x, y, z)`
- 返回值：`number`
- 用途：读取方块元数据。

#### isLoaded

- 语法：`world.isLoaded(x, y, z)`
- 返回值：`boolean`
- 用途：检查该坐标所在方块是否已经加载。

#### hasTileEntity

- 语法：`world.hasTileEntity(x, y, z)`
- 返回值：`boolean`
- 用途：检查该坐标上的方块是否提供 Tile 实体。

#### getTileNBT

- 语法：`world.getTileNBT(x, y, z)`
- 返回值：`table` 或 `nil`
- 用途：将目标位置的 Tile 实体序列化为 Lua 可读的 NBT 风格表。

如果该位置没有 Tile 实体，调用会返回 `nil`。

#### setTileNBT

- 语法：`world.setTileNBT(x, y, z, nbt)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：用 Lua 表提供的内容覆盖该 Tile 实体的 NBT 数据。

源码中的失败原因包括：

- `"no tile entity"`
- `"nbt tag compound expected, got '<type>'"`

#### getLightOpacity

- 语法：`world.getLightOpacity(x, y, z)`
- 返回值：`number`
- 用途：读取方块的遮光值。

#### getLightValue

- 语法：`world.getLightValue(x, y, z)`
- 返回值：`number`
- 用途：读取方块的发光值。

#### canSeeSky

- 语法：`world.canSeeSky(x, y, z)`
- 返回值：`boolean`
- 用途：检查该坐标是否直接朝向天空，没有被上方方块遮挡。

### 方块修改

#### setBlock

- 语法：`world.setBlock(x, y, z, id, meta)`
- 返回值：`boolean`
- 用途：在指定坐标放置单个方块。

参数说明：

- `id:number|string`：数字方块 ID 或注册方块名称。
- `meta:number`：要写入的元数据值。

#### setBlocks

- 语法：`world.setBlocks(x1, y1, z1, x2, y2, z2, id, meta)`
- 返回值：没有直接返回值
- 用途：把两个角点之间的整个轴对齐区域全部填充为指定方块。

源码会对两个端点之间的所有坐标执行包含式遍历。

### 物品与流体操作

#### insertItem

- 语法：`world.insertItem(id, count, damage, nbtJson, x, y, z, side)`
- 返回值：
  - 成功：`true`
  - 失败：`false` 或 `nil, reason`
- 用途：向指定位置的库存中插入一个物品栈。

参数说明：

- `id:string`：物品注册 ID。
- `count:number`：希望插入的数量。
- `damage:number`：损伤值或元数据。
- `nbtJson:string`：可选的 JSON 格式 NBT 字符串；没有时传 `""`。
- `x:number`、`y:number`、`z:number`：目标方块坐标。
- `side:number`：插入方向。

行为说明：

- 非法物品 ID 会直接抛出 `"invalid item id"`。
- 如果目标处没有库存，会返回 `nil, "no inventory"`。
- 当目标允许插入时，源码会继续尝试，直到请求数量尽量插完，或者再也无法继续插入为止。

#### removeItem

- 语法：`world.removeItem(x, y, z, slot[, count])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, reason`
- 用途：从指定库存槽位移除物品，并返回实际移除的数量。

源码中的失败原因是 `"no inventory"`。

#### insertFluid

- 语法：`world.insertFluid(id, amount, x, y, z, side)`
- 返回值：
  - 成功：`number`
  - 失败：`nil, reason`
- 用途：向指定储罐插入流体，并返回实际接受的流体量。

行为说明：

- 非法流体 ID 会直接抛出 `"invalid fluid id"`。
- 如果目标处没有流体处理器，会返回 `nil, "no tank"`。

#### removeFluid

- 语法：`world.removeFluid(amount, x, y, z, side)`
- 返回值：
  - 成功：被抽出的流体栈数据
  - 失败：`nil, reason`
- 用途：从指定方向的储罐中抽取流体。

源码中的失败原因是 `"no tank"`。

示例：

```lua
local world = debug.getWorld()
print("种子:", world.getSeed())
print("出生点:", world.getSpawnPoint())

local ok, reason = world.setTileNBT(0, 64, 0, {energy = 5000})
if ok == nil then
  print("NBT 写入失败:", reason)
end
```

## 相关内容

- `component`
- `computer`
- `linked card`
