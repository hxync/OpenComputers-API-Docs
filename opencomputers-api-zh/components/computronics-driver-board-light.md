# light_board

## 概述

`light_board` 是 Computronics 提供的机架式指示灯板组件。每个灯位都可以单独设置颜色与开关状态，适合做机架状态面板、报警指示、仪表板和紧凑型控制台。

这个灯板的布局不是固定的。根据机架方块上当前选择的硬件模式，组件会暴露不同数量的灯位。

## 可用性

- 仓库：`computronics`
- 组件名：`light_board`
- 常见宿主：安装在机架中的 Computronics 灯板

## 用法

```lua
local component = require("component")
local board = component.light_board

if not board then
  error("未安装 light_board 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 面板布局

可操作的灯位数量取决于当前物理模式：

- 默认模式为 `4` 个灯
- 常规模式为 `5` 个灯
- 双排五灯模式为 `10` 个灯
- 窄条十二灯模式为 `12` 个灯
- 全矩阵模式为 `42` 个灯

模式需要通过工具在机架上直接操作该板卡来切换。Lua 代码可以读取当前灯位数量，但不能直接切换布局。

Lua 里的灯位索引始终从 `1` 开始。

## 接口

### light_count

- 语法：`board.light_count()`
- 返回值：`number`
- 用途：返回当前灯板布局下可用的灯位数量。

示例：

```lua
print("灯位数量:", board.light_count())
```

### setColor

- 语法：`board.setColor(index, color)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：设置某个灯位保存的 RGB 颜色。

参数说明：

- `index:number`：灯位索引，从 `1` 开始。
- `color:number`：`0xRRGGBB` 形式的 24 位 RGB 颜色值。

行为说明：

- 合法颜色范围是 `0` 到 `16777215`，也就是 `0xFFFFFF`。
- 修改颜色会消耗组件网络能量。
- 即使该灯当前关闭，颜色也会被保存下来。

常见失败返回：

- `false, "not enough energy"`
- `false, "number must be between 0 and 16777215"`

错误情况：

- 非法灯位索引会抛出 `index out of range`

示例：

```lua
local ok, reason = board.setColor(1, 0x33CCFF)
if not ok then
  io.stderr:write("设置颜色失败: " .. tostring(reason) .. "\n")
end
```

### getColor

- 语法：`board.getColor(index)`
- 返回值：`number`
- 用途：返回某个灯位当前保存的颜色值。

参数说明：

- `index:number`：灯位索引，从 `1` 开始。

无论灯当前是否点亮，这个接口都会返回该灯位保存的颜色。

错误情况：

- 非法灯位索引会抛出 `index out of range`

示例：

```lua
print(string.format("1 号灯颜色: 0x%06X", board.getColor(1)))
```

### setActive

- 语法：`board.setActive(index, active)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：打开或关闭某个灯位。

参数说明：

- `index:number`：灯位索引，从 `1` 开始。
- `active:boolean`：`true` 表示点亮，`false` 表示关闭。

行为说明：

- 修改开关状态会消耗组件网络能量。
- 灯保持点亮时，每个游戏刻还会持续消耗维护能量。
- 如果机架无法继续支付这部分维护能量，已点亮的灯会被自动关闭。

常见失败返回：

- `false, "not enough energy"`

错误情况：

- 非法灯位索引会抛出 `index out of range`

示例：

```lua
assert(board.setColor(1, 0x00FF00))
assert(board.setActive(1, true))
```

### isActive

- 语法：`board.isActive(index)`
- 返回值：`boolean`
- 用途：返回某个灯当前是否处于点亮状态。

参数说明：

- `index:number`：灯位索引，从 `1` 开始。

错误情况：

- 非法灯位索引会抛出 `index out of range`

示例：

```lua
if board.isActive(1) then
  print("1 号灯当前处于点亮状态")
end
```

## 示例

下面的脚本会把整块灯板全部设置为红色，并依次点亮全部灯位。

```lua
local count = board.light_count()

for i = 1, count do
  assert(board.setColor(i, 0xFF3333))
  assert(board.setActive(i, true))
end
```

## 说明

- 颜色修改和开关修改都会消耗组件网络缓冲能量。
- 如果你希望长期点亮大量灯位，需要保证机架持续供能，因为激活状态会不断消耗维护能量。

## 相关内容

- `component`
- `rack_capacitor`
- `switch_board`
