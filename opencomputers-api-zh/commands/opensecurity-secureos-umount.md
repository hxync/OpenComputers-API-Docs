# umount

## 概述

按挂载路径或文件系统身份卸载已挂载的文件系统。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\umount.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\umount`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\umount`

## 语法

```lua
umount MOUNT
```
```lua
umount -a FILESYSTEM
```

## 用途

不带 `-a` 时，参数必须解析为一个精确的挂载点路径。带 `-a` 时，参数会被当作文件系统标签或地址处理，只要能唯一匹配，就允许使用地址前缀。

## 参数

- `MOUNT`: 路径模式下要卸载的挂载点路径。
- `FILESYSTEM`: 配合 `-a` 使用的文件系统标签或地址。

## 选项

- `-a`: 按文件系统标签或地址卸载，而不是按精确挂载点路径卸载。

## 示例

```lua
umount /mnt/disk
```
卸载精确挂载在 `/mnt/disk` 上的文件系统。

```lua
umount -a e4f3
```
卸载标签或缩写地址可解析为 `e4f3` 的文件系统。

## 备注

- 路径模式不会接受“位于某个挂载内部”的普通路径；参数本身必须正好就是挂载点。
- 如果没有找到匹配的挂载，命令会输出 `nothing to unmount here`。
