# enderstorage

## 概述

本页说明 OpenComputers 对 EnderStorage 频率型容器的集成接口。
这些接口允许 Lua 读取当前频道频率、切换频率，以及判断当前频道属于某个玩家还是公共命名空间。

## 可用性

- 依赖：`enderstorage`
- 标签：`integration-required`

## 组件族

这些接口由使用 EnderStorage 频率机制的环境暴露，例如带有频率设置的末影存储方块或物品。

## 通用行为

- 频率以底层 EnderStorage 实现使用的打包数值形式返回。
- 这个数值在实际意义上对应当前三色频道组合，以及它所处的公共或私有命名空间。
- 私有频道通常会返回所属玩家名。
- 公共频道通常会返回 `global`。

## 接口

### `getFrequency`

- 语法：`component.getFrequency()`
- 返回：数字
- 用途：读取当前 EnderStorage 频率值

当你准备切换频道，或者需要先记录当前频道时，这个接口最直接。

示例：

```lua
local frequency = storage.getFrequency()
print("Current frequency:", frequency)
```

### `setFrequency`

- 语法：`component.setFrequency(value)`
- 参数：
  - `value`：要设置的数值频率
- 返回：布尔值
- 用途：把目标切换到另一个 EnderStorage 频道

只要底层目标接受这个值，频率就会立即切换。

示例：

```lua
local ok = storage.setFrequency(123)
print("Frequency changed:", ok)
```

### `getOwner`

- 语法：`component.getOwner()`
- 返回：字符串
- 用途：读取当前频道所属的命名空间拥有者

它可以帮助你区分这是某个玩家的私有频道，还是共享的公共频道。

示例：

```lua
print("Owner:", storage.getOwner())
```

## 完整示例

```lua
local component = require("component")
local storage = component.proxy(component.list()())

print("Owner:", storage.getOwner())
print("Current frequency:", storage.getFrequency())

local ok = storage.setFrequency(123)
print("Frequency changed:", ok)
```

## 相关内容

- `modem`
- `inventory_controller`
