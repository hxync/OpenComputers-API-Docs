# mount.cow

## 概述

挂载一个由只读源和写层组合而成的写时复制覆盖文件系统。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\mount.cow.lua`

## 语法

```lua
mount.cow
```
```lua
mount.cow READ_FS WRITE_FS PATH
```

## 用途

不带参数时，命令列出当前挂载表。提供三个参数时，它会解析两个文件系统代理，使用 `pipes.cowProxy` 组装成写时复制覆盖层，再把该覆盖文件系统挂载到目标路径。

## 参数

- `READ_FS`: 下层只读源文件系统的标签或地址。
- `WRITE_FS`: 记录修改内容的上层可写文件系统标签或地址。
- `PATH`: 合并后的视图要挂载到的目录。

## 示例

```lua
mount.cow
```
输出当前挂载表。

```lua
mount.cow 123 456 /mnt/work
```
把文件系统 `123` 作为只读底层、文件系统 `456` 作为写层，在 `/mnt/work` 挂载一个写时复制视图。

## 备注

- 只要 `filesystem.proxy` 能唯一解析，组件地址就可以使用缩写前缀。
- 注意：这些地址可以使用缩写形式，只要能够唯一匹配。
