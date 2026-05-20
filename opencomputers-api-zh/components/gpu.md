# gpu

## 概述

`gpu` 组件是 OpenComputers 的文本图形接口。它负责屏幕绑定、颜色与调色板、分辨率、视口大小、文本输出、矩形复制和填充，以及离屏 VRAM 缓冲区管理。

很多接口都是围绕“**当前活动缓冲区**”工作的：

- 缓冲区 `0` 永远表示已绑定的屏幕；
- `1` 及以上表示由 GPU 自己分配的 VRAM 缓冲区。

当当前目标是真实屏幕时，调用会消耗正常的 GPU 能量和调用预算；当目标是离屏缓冲区时，许多写操作会更便宜，而且即使没有绑定屏幕也能工作。

## 可用性

- 仓库：`opencomputers`
- 组件名：`gpu`
- 常见宿主：安装了显卡的电脑、机器人、平板、微控制器

## 用法

```lua
local component = require("component")
local gpu = component.gpu

if not gpu then
  error("当前环境未安装 gpu 组件")
end
```

## 核心概念

- `bind(address[, reset])` 用于把 GPU 连接到屏幕。
- `setActiveBuffer(index)` 用于决定后续绘制写到屏幕 `0`，还是写到 VRAM 缓冲区 `1+`。
- `bitblt(...)` 用于在缓冲区之间复制矩形区域，也是把离屏页渲染回屏幕的主要方式。
- Lua 传入的坐标全部是 `1` 基坐标。

## 缓冲区管理

### getActiveBuffer

- 语法：`gpu.getActiveBuffer()`
- 返回值：`number`
- 用途：读取当前选中的目标缓冲区索引。

`0` 永远代表屏幕槽位，即使当前实际上没有绑定屏幕，也可能处于活动状态。

### setActiveBuffer

- 语法：`gpu.setActiveBuffer(index)`
- 返回值：
  - 成功：之前的缓冲区索引
  - 失败：`nil, reason`
- 用途：切换后续绘图操作要写入的目标缓冲区。

参数说明：

- `index:number`：`0` 表示已绑定的屏幕，`1+` 表示之前分配过的 VRAM 缓冲区。

源码中的失败原因是 `"invalid buffer index"`。

### buffers

- 语法：`gpu.buffers()`
- 返回值：`table`
- 用途：列出当前已分配的 VRAM 缓冲区索引。

### allocateBuffer

- 语法：`gpu.allocateBuffer([width, height])`
- 返回值：
  - 成功：`number`
  - 失败：`nil, reason`
- 用途：分配一个新的离屏 VRAM 缓冲区，并返回它的索引。

参数说明：

- `width:number` 可选：缓冲区宽度。默认使用 GPU 可支持的最大宽度。
- `height:number` 可选：缓冲区高度。默认使用 GPU 可支持的最大高度。

行为说明：

- 缓冲区 `0` 保留给屏幕，所以新分配的缓冲区索引一定从 `1` 开始。
- 即使当前没有绑定屏幕，也可以分配 VRAM 缓冲区。
- 源码中的失败原因包括：
  - `"invalid page dimensions: must be greater than zero"`
  - `"not enough video memory"`
  - `"graphics card appears disconnected"`

### freeBuffer

- 语法：`gpu.freeBuffer([index])`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：释放一个 VRAM 缓冲区。

参数说明：

- `index:number` 可选：要释放的缓冲区索引，默认是当前活动缓冲区。

如果被释放的是当前活动缓冲区，活动索引会自动退回到 `0`。

源码中的失败原因是 `"no buffer at index"`。

### freeAllBuffers

- 语法：`gpu.freeAllBuffers()`
- 返回值：`number`
- 用途：释放全部 VRAM 缓冲区，并返回实际释放的数量。

如果当前活动缓冲区也是其中之一，活动索引会自动变回 `0`。

### totalMemory

- 语法：`gpu.totalMemory()`
- 返回值：`number`
- 用途：读取可用于 GPU 缓冲区的总 VRAM 容量。

这个值不包含屏幕本身占用的存储。

### freeMemory

- 语法：`gpu.freeMemory()`
- 返回值：`number`
- 用途：读取当前尚未被缓冲区占用的 VRAM 余量。

### getBufferSize

- 语法：`gpu.getBufferSize([index])`
- 返回值：
  - 成功：`number, number`
  - 失败：`nil, reason`
- 用途：读取指定缓冲区的宽度和高度。

参数说明：

- `index:number` 可选：要查询的缓冲区索引，默认是当前活动缓冲区。

说明：

- 传入 `0` 时，返回的是当前绑定屏幕的分辨率。
- 源码中的失败原因是 `"invalid buffer index"`。

示例：

```lua
local id = gpu.allocateBuffer(80, 25)
print("新缓冲区:", id)
print("尺寸:", gpu.getBufferSize(id))
```

## 缓冲区复制

### bitblt

- 语法：`gpu.bitblt([dst, col, row, width, height, src, fromCol, fromRow])`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：把一个缓冲区中的矩形区域复制到另一个缓冲区。

参数说明：

- `dst:number` 可选：目标缓冲区索引，默认是屏幕缓冲区 `0`。
- `col:number` 可选：目标列，默认 `1`。
- `row:number` 可选：目标行，默认 `1`。
- `width:number` 可选：复制宽度，默认使用目标宽度。
- `height:number` 可选：复制高度，默认使用目标高度。
- `src:number` 可选：源缓冲区索引，默认是当前活动缓冲区。
- `fromCol:number` 可选：源起始列，默认 `1`。
- `fromRow:number` 可选：源起始行，默认 `1`。

行为说明：

- 它是把离屏 VRAM 缓冲区内容渲染回屏幕的主要方式。
- 在两个 VRAM 缓冲区之间复制，本质上属于显存到显存复制。
- 把“脏”的 VRAM 页复制到屏幕时开销较大，源码里还可能触发调用预算限流。
- 失败通常来自底层缓冲区查找或能量不足，常见原因包括：
  - `"no screen"`
  - `"invalid buffer index"`
  - `"not enough energy"`

示例：

```lua
local page = assert(gpu.allocateBuffer(40, 10))
gpu.setActiveBuffer(page)
gpu.fill(1, 1, 40, 10, " ")
gpu.set(2, 2, "离屏缓冲已准备")
gpu.bitblt(0, 1, 1, 40, 10, page, 1, 1)
gpu.setActiveBuffer(0)
```

## 屏幕绑定

### bind

- 语法：`gpu.bind(address[, reset])`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：把 GPU 绑定到指定的屏幕组件。

参数说明：

- `address:string`：屏幕组件地址。
- `reset:boolean` 可选：默认值为 `true`。

当 `reset` 为 `true` 时，源码会执行以下重置动作：

- 分辨率重置为 GPU 最大分辨率和屏幕最大分辨率的共同最小值；
- 色深重置为 GPU 与屏幕共同支持的最高色深；
- 前景色重置为白色；
- 背景色重置为黑色；
- 如果屏幕端支持显存页栅格化，会清空该屏幕注册过的 VRAM 页。

源码中的失败原因包括：

- `"invalid address"`
- `"not a screen"`

### getScreen

- 语法：`gpu.getScreen()`
- 返回值：
  - 成功：`string`
  - 失败：`nil, reason`
- 用途：读取当前已绑定屏幕的组件地址。

源码中的失败原因是 `"no screen"`。

## 颜色和调色板

### getBackground

- 语法：`gpu.getBackground()`
- 返回值：`number, boolean`
- 用途：读取当前背景色，以及这个背景色是否来自调色板。

### setBackground

- 语法：`gpu.setBackground(value[, palette])`
- 返回值：`number[, number]`
- 用途：设置后续绘制使用的背景色。

参数说明：

- `value:number`：原始 RGB 值，或在 `palette` 为 `true` 时表示调色板索引。
- `palette:boolean` 可选：为 `true` 时，把 `value` 按调色板索引解释。

返回值说明：

- 第一个返回值：之前生效的 RGB 颜色值；
- 第二个返回值：如果旧颜色来自调色板，则返回旧调色板索引，否则为 `nil`。

### getForeground

- 语法：`gpu.getForeground()`
- 返回值：`number, boolean`
- 用途：读取当前前景色，以及这个前景色是否来自调色板。

### setForeground

- 语法：`gpu.setForeground(value[, palette])`
- 返回值：`number[, number]`
- 用途：设置后续绘制使用的前景色。

它的参数和返回值行为与 `setBackground` 相同。

### getPaletteColor

- 语法：`gpu.getPaletteColor(index)`
- 返回值：`number`
- 用途：读取一个调色板槽位的颜色值。

参数说明：

- `index:number`：调色板槽位索引。

无效索引会直接抛出 `"invalid palette index"`。

### setPaletteColor

- 语法：`gpu.setPaletteColor(index, color)`
- 返回值：`number`
- 用途：修改一个调色板槽位，并返回旧颜色值。

参数说明：

- `index:number`：调色板槽位索引。
- `color:number`：新的 RGB 颜色值。

无效索引会直接抛出 `"invalid palette index"`。

### getDepth

- 语法：`gpu.getDepth()`
- 返回值：`number`
- 用途：读取当前实际使用的色深位数。

### setDepth

- 语法：`gpu.setDepth(depth)`
- 返回值：之前的色深
- 用途：修改当前色深。

实际可接受的值取决于显卡等级和屏幕能力，源码接受：

- `1`
- `4`
- `8`

不受支持的请求会直接抛出 `"unsupported depth"`。

### maxDepth

- 语法：`gpu.maxDepth()`
- 返回值：`number`
- 用途：读取 GPU 和当前屏幕共同支持的最高色深。

示例：

```lua
local oldBg = gpu.setBackground(0x0000FF)
local oldFg = gpu.setForeground(0xFFFFFF)
print("旧颜色:", oldBg, oldFg)
```

## 分辨率和视口

### getResolution

- 语法：`gpu.getResolution()`
- 返回值：`number, number`
- 用途：读取当前屏幕缓冲区的文本分辨率。

### setResolution

- 语法：`gpu.setResolution(width, height)`
- 返回值：`boolean`
- 用途：修改屏幕分辨率。

参数说明：

- `width:number`
- `height:number`

请求值必须同时满足 GPU 等级限制和已绑定屏幕的限制。非法值会直接抛出 `"unsupported resolution"`。

### maxResolution

- 语法：`gpu.maxResolution()`
- 返回值：`number, number`
- 用途：读取 GPU 和当前屏幕共同支持的最大分辨率。

### getViewport

- 语法：`gpu.getViewport()`
- 返回值：`number, number`
- 用途：读取当前视口大小。

视口代表当前用于显示的可见区域，它不能大于当前分辨率。

### setViewport

- 语法：`gpu.setViewport(width, height)`
- 返回值：`boolean`
- 用途：修改视口大小。

请求的视口必须同时满足 GPU 限制，并且不能超过当前屏幕分辨率。非法值会直接抛出 `"unsupported viewport size"`。

## 读取与绘制

### get

- 语法：`gpu.get(x, y)`
- 返回值：`string, number, number, number|nil, number|nil`
- 用途：读取当前活动缓冲区中某一个单元格的内容。

返回值依次为：

- 该位置的字符；
- 解析后的前景 RGB 颜色；
- 解析后的背景 RGB 颜色；
- 如果前景色来自调色板，则返回前景调色板索引，否则为 `nil`；
- 如果背景色来自调色板，则返回背景调色板索引，否则为 `nil`。

### set

- 语法：`gpu.set(x, y, value[, vertical])`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：向当前活动缓冲区写入一个字符串。

参数说明：

- `x:number`、`y:number`：起始位置。
- `value:string`：要绘制的文本。
- `vertical:boolean` 可选：为 `true` 时，按纵向逐字符写入。

源码中的失败原因是 `"not enough energy"`。

### copy

- 语法：`gpu.copy(x, y, width, height, tx, ty)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：在当前活动缓冲区内部复制一个矩形区域，并按给定偏移量移动。

参数说明：

- `x:number`、`y:number`：源区域起点。
- `width:number`、`height:number`：源区域尺寸。
- `tx:number`、`ty:number`：平移偏移量。

源码中的失败原因是 `"not enough energy"`。

### fill

- 语法：`gpu.fill(x, y, width, height, char)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：用一个字符填充当前活动缓冲区中的矩形区域。

参数说明：

- `char:string`：必须恰好只包含一个 Unicode 码点。

失败行为：

- 能量不足时返回 `nil, "not enough energy"`；
- 当 `char` 不是单个字符时，会直接抛出 `"invalid fill value"`。

示例：

```lua
gpu.bind(component.screen.address)
gpu.setResolution(80, 25)
gpu.setBackground(0x000000)
gpu.setForeground(0x00FF00)
gpu.fill(1, 1, 80, 25, " ")
gpu.set(2, 2, "系统已启动")
```

## 相关内容

- `screen`
- `component`
- `computer`
