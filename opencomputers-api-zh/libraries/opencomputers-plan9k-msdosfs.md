# msdos

## 概述

Plan9k 的 FAT12/FAT16 文件系统驱动辅助库，可把软盘或镜像文件暴露成文件系统代理。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\msdosfs.lua`

## 用法

```lua
local msdos = dofile("/lib/msdosfs.lua")
```

## 接口

### msdos.proxy(fatfile, fatsize)

- 说明: 从类块设备文件句柄读取 FAT 引导扇区，识别 FAT12 或 FAT16 布局，并返回一个可浏览和修改该镜像的文件系统式代理对象。
- 参数:
  - `fatfile`: 指向 FAT 镜像或块设备的已打开文件句柄。
  - `fatsize`: 可选，强制指定 FAT 位宽，例如 `12` 或 `16`，用于跳过自动识别。

```lua
local proxy = msdos.proxy(io.open('/dev/fd0', 'rb'))
```

## 备注

- 对外公开的入口是 `msdos.proxy`；源码中的大量 `_msdos.*` 函数都属于内部解析和 FAT 表操作辅助逻辑。
