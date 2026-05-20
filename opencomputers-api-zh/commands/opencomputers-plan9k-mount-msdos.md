# mount.msdos

## 概述

通过 Plan9k 的 `msdosfs` 驱动挂载 DOS FAT 文件系统镜像。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\mount.msdos.lua`

## 语法

```lua
mount.msdos
```
```lua
mount.msdos BLOCKDEVICE PATH
```

## 用途

不带参数时，命令列出当前挂载表。提供块设备文件路径和挂载点时，它会创建一个 `msdosfs` 代理，并把该代理挂到文件系统树中。

## 参数

- `BLOCKDEVICE`: 保存 FAT 格式磁盘镜像的底层文件路径。
- `PATH`: 要把 FAT 文件系统挂载到的目录。

## 示例

```lua
mount.msdos /tmp/floppy.img /mnt/floppy
```
把 `/tmp/floppy.img` 中的 FAT 镜像挂载到 `/mnt/floppy`。

## 备注

- 这个实现固定按 FAT12 方式创建驱动代理，因此主要面向同一套 Plan9k 工具链生成的镜像，例如由 `mkdosfs` 格式化出的镜像文件。
- 注意：这些地址可以使用缩写形式，只要能够唯一匹配。
