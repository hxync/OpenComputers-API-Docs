# mkdosfs

## 概述

在块设备文件中创建一个小型 FAT12 或 FAT16 文件系统。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\mkdosfs.lua`

## 语法

```lua
mkdosfs IMAGE_FILE
```

## 用途

以二进制写模式打开目标文件，把它视为 512 字节扇区的块设备，根据总大小计算 FAT 布局参数，写入引导记录，初始化两份 FAT 表，并清空根目录区域。

## 参数

- `IMAGE_FILE`: 现有的可写文件，会被直接覆写成 FAT 文件系统布局。

## 示例

```lua
mkdosfs /tmp/floppy.img
```
把 `/tmp/floppy.img` 格式化成 DOS 文件系统镜像。

## 备注

- 命令不会帮你新建镜像文件；它要求目标文件事先已经存在且可写。
- 目标文件中原有的全部内容都会被破坏。
