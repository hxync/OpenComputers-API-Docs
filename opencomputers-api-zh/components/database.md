# database

## 概述

`database` 组件用于存储“幽灵物品栈”记录，供其他组件后续引用。它特别适合处理那些仅靠物品 ID 还不够、还需要区分损伤值或 NBT 的场景，例如库存筛选、地质分析仪存方块、物品比对等。

数据库里不保存真实物品，只保存物品描述数据。其他组件可以通过槽位或计算出的哈希值来引用这些条目。

## 可用性

- 仓库：`opencomputers`
- 组件名：`database`
- 常见宿主：数据库升级、数据库方块，或其他会暴露 `database` 组件的硬件

## 用法

```lua
local component = require("component")
local database = component.database

if not database then
  error("当前环境没有 database 组件")
end
```

## 接口

### get

- 语法：`database.get(slot)`
- 返回值：`table` 或 `nil`
- 用途：读取指定数据库槽位里保存的物品表示。

参数说明：

- `slot:number`：数据库槽位编号。

如果该槽位为空，调用会返回 `nil`。

示例：

```lua
local entry = database.get(1)
if entry then
  print("已存物品:", entry.name, entry.damage)
else
  print("1 号槽位为空")
end
```

### computeHash

- 语法：`database.computeHash(slot)`
- 返回值：`string` 或 `nil`
- 用途：为指定槽位中的物品记录计算一个稳定的 SHA-256 哈希值。

参数说明：

- `slot:number`：数据库槽位编号。

这个哈希值通常和 `indexOf` 配合使用，方便之后重新定位相同条目。

如果该槽位为空，调用会返回 `nil`。

示例：

```lua
local hash = database.computeHash(1)
if hash then
  print("哈希值:", hash)
end
```

### indexOf

- 语法：`database.indexOf(hash)`
- 返回值：`number`
- 用途：查找第一个与指定哈希值匹配的数据库条目。

参数说明：

- `hash:string`：通常是 `computeHash` 生成的哈希值。

返回值说明：

- 找到时返回正数槽位编号；
- 没找到时返回 `-1`。

示例：

```lua
local hash = database.computeHash(1)
if hash then
  print("匹配槽位:", database.indexOf(hash))
end
```

### clear

- 语法：`database.clear(slot)`
- 返回值：`boolean`
- 用途：清空一个数据库槽位。

参数说明：

- `slot:number`：数据库槽位编号。

返回值说明：

- `true` 表示该槽位原来有内容；
- `false` 表示原来就是空槽。

示例：

```lua
local hadEntry = database.clear(4)
print("4 号槽位原来是否有数据:", hadEntry)
```

### copy

- 语法：`database.copy(fromSlot, toSlot[, address])`
- 返回值：`boolean`
- 用途：把一个已保存的条目复制到另一个槽位，也可以复制到另一块数据库。

参数说明：

- `fromSlot:number`：当前数据库中的源槽位。
- `toSlot:number`：目标槽位。
- `address:string` 可选：目标数据库组件地址。省略时表示在当前数据库内部复制。

返回值说明：

- `true` 表示目标槽位原来已经有内容；
- `false` 表示目标槽位原来为空。

示例：在同一块数据库内复制。

```lua
local replaced = database.copy(1, 2)
print("目标槽位原来是否被占用:", replaced)
```

示例：复制到另一块数据库。

```lua
local other = component.proxy("01234567-89ab-cdef-0123-456789abcdef")
database.copy(1, 1, other.address)
```

### clone

- 语法：`database.clone(address)`
- 返回值：`number`
- 用途：把当前数据库中保存的内容整体复制到另一块数据库组件。

参数说明：

- `address:string`：目标数据库地址。

返回值说明：

- 返回实际按较小数据库容量限制后，准备复制的槽位数量。

源码会在克隆后让电脑短暂停顿，因此它更适合做初始化或配置同步，而不是高频循环里的操作。

示例：

```lua
local copied = database.clone("01234567-89ab-cdef-0123-456789abcdef")
print("复制的槽位数:", copied)
```

### set

- 语法：`database.set(slot, id, damage[, nbtJson])`
- 返回值：`boolean[, string]`
- 用途：通过物品 ID、损伤值和可选 JSON NBT，直接写入一个幽灵物品栈条目。

参数说明：

- `slot:number`：目标数据库槽位。
- `id:string`：物品注册 ID，例如 `minecraft:wool`。
- `damage:number`：损伤值或元数据值。
- `nbtJson:string` 可选：JSON 格式 NBT。没有时传 `""` 或省略。

返回值说明：

- 成功时返回 `true`；
- 物品 ID 不存在时返回 `false, "invalid item id"`。

示例：

```lua
local ok, reason = database.set(1, "minecraft:wool", 14)
if not ok then
  io.stderr:write("写入失败: " .. tostring(reason) .. "\n")
end
```

带 JSON NBT 的示例：

```lua
database.set(2, "minecraft:potion", 0, "{CustomPotionEffects:[{Id:1,Amplifier:1,Duration:200}]}")
```

## 相关内容

- `component`
- `inventory_controller`
- `geolyzer`
