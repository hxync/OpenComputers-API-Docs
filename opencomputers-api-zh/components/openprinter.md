# openprinter

## 概述

`openprinter` 组件用于控制 OpenPrinter 页面打印机。它可以先在内存中累计页面内容、设置标题、打印最终页面、在配置允许时打印命名牌、扫描放入扫描槽中的已打印页面，并查询当前纸张与墨盒余量。

## 可用性

- 仓库：`openprinter`
- 组件名：`openprinter`
- 常见宿主：放置在世界中的 OpenPrinter 打印机

## 用法

```lua
local component = require("component")
local printer = component.openprinter

if not printer then
  error("未安装 openprinter 组件")
end
```

该打印机还会挂载一个名为 `printer` 的内置文件系统环境，其中包含随模组附带的 Lua 程序。

## 物品槽布局

- `0` 号槽：黑色墨盒
- `1` 号槽：彩色墨盒
- `2` 号槽：输入纸张、纸卷或命名牌
- `3` 到 `12` 号槽：输出槽
- `13` 号槽：扫描槽，用于读取已打印页面

实际影响：

- 打印页面时，必须同时装有黑墨和彩墨。
- 打印命名牌时，只需要黑墨和放在 `2` 号槽中的原版命名牌。
- `scan()` 与 `scanLine()` 只会读取 `13` 号槽中的物品。

## 页面缓冲模型

`writeln()` 并不会立刻打印，而是把内容追加到一个内存中的页面缓冲区：

- 最多 `20` 行
- 每一行同时记录：
  - 文本
  - 整数颜色
  - 对齐字符串

`setTitle()` 会为下一次打印的页面保存标题。`print()` 成功后会消耗当前缓冲页，并清空行缓冲。

## 墨水与纸张说明

- `OpenPrinter.cfg.printerInkUse` 默认是每个墨盒 `4000` 次使用量。
- 纸张输入可使用：
  - 原版纸张
  - OpenPrinter 纸卷
- 纸卷余量返回值为 `256 - itemDamage`。

## 接口

### greet

- 语法：`printer.greet()`
- 返回：`string`
- 用途：返回 OpenPrinter 内置问候语。

固定返回文本：

```text
Lasciate ogne speranza, voi ch'intrate
```

### writeln

- 语法：
  - `printer.writeln(text)`
  - `printer.writeln(text, color)`
  - `printer.writeln(text, alignment)`
  - `printer.writeln(text, color, alignment)`
- 返回：`true`
- 用途：向待打印页面缓冲区追加一行内容。

参数说明：

- `text:string`：该行文本内容。
- `color:number` 可选：行颜色，默认值是 `0x000000`。
- `alignment:string` 可选：对齐标记字符串，默认值是 `"left"`。

行为说明：

- 当缓冲区已经有 `20` 行时，会抛出 `To many lines.`。
- 源码会原样保存 alignment 字符串，不做合法性校验。

示例：

```lua
printer.writeln("季度报告", 0x3366CC, "center")
printer.writeln("第二行")
```

### setTitle

- 语法：`printer.setTitle(title)`
- 返回：`true`
- 用途：为下一张打印页面设置标题。

参数说明：

- `title:string`：下一张打印页要使用的页面标题和显示名称。

行为说明：

- 这个标题会在下一次 `print()` 成功时被消耗。

示例：

```lua
printer.setTitle("会议记录")
```

### print

- 语法：`printer.print([unused, copies])`
- 返回：
  - 成功：`true`
  - 兜底路径：`false`
  - 失败：抛出异常
- 用途：在第一个空输出槽中生成一张已打印页面。

参数说明：

- `unused:any` 可选：源码里读取但没有实际用途。
- `copies:number` 可选：从第二个参数读取，默认值是 `9`。

重要行为：

- 当前实现会在生成第一页后立即返回，所以即使传入更大的 copies，一次调用实际上也只会产出一张页面。
- 成功打印后，当前缓冲的行、颜色和对齐数据都会被清空。
- 已设置的标题也会在成功打印后被清空。

所需耗材：

- `0` 号槽必须有黑墨
- `1` 号槽必须有彩墨
- `2` 号槽必须有纸或纸卷
- `3` 到 `12` 号槽中至少要有一个空槽

常见失败：

- `Please load Ink.`
- `Please load Paper.`
- `No empty output slots.`

示例：

```lua
printer.clear()
printer.setTitle("备忘录")
printer.writeln("你好，世界")
assert(printer.print())
```

### printTag

- 语法：`printer.printTag(name)`
- 返回：`true`
- 用途：在第一个空输出槽中打印一个命名后的原版命名牌。

参数说明：

- `name:string`：要写入命名牌显示名的文本。

前提条件：

- OpenPrinter 配置中的 `enableNameTag` 必须启用
- `0` 号槽需要黑墨
- `2` 号槽需要放入原版命名牌
- `3` 到 `12` 号槽中至少要有一个空槽

常见失败：

- `Name Tag printing is disabled.`
- `Please load Black Ink.`
- `Please load Name Tags.`
- `No empty output slots.`

示例：

```lua
assert(printer.printTag("仓库钥匙"))
```

### scanLine

- 语法：`printer.scanLine(index)`
- 返回：
  - 成功：`string`
  - 扫描槽中物品不合法：`false`
  - 扫描槽为空：回调报错
- 用途：读取扫描槽 `13` 中已打印页面的某一行原始内容。

参数说明：

- `index:number`：要读取的行号。

行为说明：

- 打印页中的行键名是 `line0` 到 `line20`。
- 如果 `13` 号槽里放的是非 `PrintedPage` 或没有对应 NBT 的页面物品，则返回 `false`。
- 如果 `13` 号槽本身为空，当前实现会先直接解引用该槽位物品，因此会报错，而不是稳定返回 `false`。

示例：

```lua
local line = printer.scanLine(0)
print(line)
```

### scan

- 语法：`printer.scan()`
- 返回：
  - 成功：`title, lines`
  - 扫描槽中物品不合法：`false`
  - 扫描槽为空：回调报错
- 用途：读取扫描槽 `13` 中的已打印页面。

返回值说明：

- `title:string|nil`：页面标题
- `lines:table`：按数字索引保存的原始行字符串表

行为说明：

- 返回的行内容与 NBT 中保存的值完全一致。
- 每个原始行字符串内部带有分隔符，拼接了：
  - 文本
  - 颜色
  - 对齐方式
- 如果 `13` 号槽里放入的不是打印页，则返回 `false`。
- 如果 `13` 号槽为空，源码会在判定前先访问该物品栈，因此会直接报错，而不是返回 `false`。

示例：

```lua
local title, lines = printer.scan()
if title ~= false then
  print(title)
  for index, raw in pairs(lines) do
    print(index, raw)
  end
end
```

### getPaperLevel

- 语法：`printer.getPaperLevel()`
- 返回：
  - 有纸张输入时：`number`
  - 缺失或物品不合法时：`false`
- 用途：返回 `2` 号输入槽中的剩余纸张数量。

行为说明：

- 如果是纸卷，返回 `256 - itemDamage`。
- 如果是普通纸堆，返回当前堆叠数量。

### getBlackInkLevel

- 语法：`printer.getBlackInkLevel()`
- 返回：
  - 有效黑墨盒：`number`
  - 缺失或物品错误：`false`
- 用途：返回黑色墨盒剩余可用次数。

行为说明：

- 返回值是 `printerInkUse - itemDamage`。

### getColorInkLevel

- 语法：`printer.getColorInkLevel()`
- 返回：
  - 有效彩墨盒：`number`
  - 缺失或物品错误：`false`
- 用途：返回彩色墨盒剩余可用次数。

行为说明：

- 返回值是 `printerInkUse - itemDamage`。

### charCount

- 语法：`printer.charCount(text)`
- 返回：`number`
- 用途：统计可见字符数量，并忽略 Minecraft 格式控制码。

参数说明：

- `text:string`：要统计的文本。

行为说明：

- 实现会先去掉匹配 `§` 颜色/样式代码的格式串，再计算剩余长度。

示例：

```lua
print(printer.charCount("§aGreen Text"))
```

### clear

- 语法：`printer.clear()`
- 返回：`true`
- 用途：清空待打印的行缓冲、颜色缓冲、对齐缓冲以及待用标题。

示例：

```lua
printer.clear()
```

## 示例

```lua
printer.clear()
printer.setTitle("状态报告")
printer.writeln("系统已上线", 0x00AA00, "left")
printer.writeln("所有节点均可达", 0x000000, "left")

print("Paper:", printer.getPaperLevel())
print("Black ink:", printer.getBlackInkLevel())
print("Color ink:", printer.getColorInkLevel())

assert(printer.print())
```

## 相关内容

- `component`
- `openprinter-openprinter-xerox`
