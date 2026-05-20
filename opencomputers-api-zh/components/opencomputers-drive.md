# drive

## 概述

`drive` 组件提供的是 OpenComputers 硬盘和类似块设备的底层扇区/字节访问接口。它比普通的 `filesystem` 组件更底层：你面对的不是文件和目录，而是原始字节和扇区。

这个接口主要适合做自定义格式、引导程序、磁盘检查、取证工具以及各种底层存储实验。

## 可用性

- 仓库：`opencomputers`
- 组件名：`drive`
- 常见宿主：硬盘和兼容的原始块设备

## 重要说明

- Lua 侧看到的扇区号和字节偏移都是 `1` 基编号。
- 每个扇区固定是 `512` 字节。
- 读写调用会消耗调用预算，并且在虚拟磁头需要寻道时可能让电脑暂停。
- 某些驱动器可能是只读的，此时写操作会被拒绝。

## 用法

```lua
local component = require("component")
local drive = component.drive

if not drive then
  error("当前环境没有 drive 组件")
end
```

## 接口

### getLabel

- 语法：`drive.getLabel()`
- 返回值：`string` 或 `nil`
- 用途：读取当前驱动器标签。

有些驱动器实现可能不支持标签功能，此时这里可能返回 `nil`。

### setLabel

- 语法：`drive.setLabel(value)`
- 返回值：`string`
- 用途：修改驱动器标签，并返回最终生效的标签。

参数说明：

- `value:string`：希望设置的新标签。

源码里可能抛出的错误包括：

- `"drive is read only"`
- `"drive does not support labeling"`

### getCapacity

- 语法：`drive.getCapacity()`
- 返回值：`number`
- 用途：读取驱动器总原始容量，单位为字节。

### getSectorSize

- 语法：`drive.getSectorSize()`
- 返回值：`number`
- 用途：读取单个扇区大小，单位为字节。

这个值固定为 `512`。

### getPlatterCount

- 语法：`drive.getPlatterCount()`
- 返回值：`number`
- 用途：读取驱动器模拟的盘片数量。

它主要影响底层寻道行为和模拟细节，对普通文件存储脚本通常没有决定性影响。

### readSector

- 语法：`drive.readSector(sector)`
- 返回值：`string`
- 用途：读取一个完整扇区。

参数说明：

- `sector:number`：扇区号，从 `1` 开始。

行为说明：

- 返回值是一个长度为 `512` 字节的 Lua 字符串；
- 非法扇区号会抛出 `"invalid offset, not in a usable sector"`。

示例：

```lua
local raw = drive.readSector(1)
print("返回字节数:", #raw)
```

### writeSector

- 语法：`drive.writeSector(sector, value)`
- 返回值：没有直接返回值
- 用途：向一个扇区写入原始数据。

参数说明：

- `sector:number`：扇区号，从 `1` 开始。
- `value:string`：要写入的原始字节内容。

行为说明：

- 目标驱动器不能是只读；
- 如果 `value` 少于 512 字节，则只会覆盖该扇区开头对应长度的部分，剩余字节保持原状；
- 非法扇区号会抛出 `"invalid offset, not in a usable sector"`。

源码里常见的写入失败错误是 `"drive is read only"`。

### readByte

- 语法：`drive.readByte(offset)`
- 返回值：`number`
- 用途：按原始字节偏移读取一个字节。

参数说明：

- `offset:number`：字节偏移，从 `1` 开始。

重要细节：

- 源码底层使用的是有符号 Java `byte` 数组，因此这里返回的数值可能会落在 `-128..127` 范围；
- 如果你想得到无符号 `0..255`，可以在 Lua 里自行归一化：

```lua
local b = drive.readByte(1)
local unsigned = (b + 256) % 256
```

### writeByte

- 语法：`drive.writeByte(offset, value)`
- 返回值：没有直接返回值
- 用途：按原始字节偏移写入一个字节。

参数说明：

- `offset:number`：字节偏移，从 `1` 开始。
- `value:number`：要写入的字节值。

源码里常见的写入失败错误是 `"drive is read only"`。

### 原始访问示例

```lua
local sector = drive.readSector(1)
local first = drive.readByte(1)
print("扇区大小:", drive.getSectorSize())
print("首字节的有符号值:", first)
drive.writeByte(1, 0x42)
```

## 相关内容

- `filesystem`
- `eeprom`
- `component`
