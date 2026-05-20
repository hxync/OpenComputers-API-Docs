# mount

## 概述

列出当前挂载，或把文件系统挂接到 OpenOS 目录树中。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\mount.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\mount`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\mount`

## 语法

```lua
mount
```
```lua
mount LABEL PATH
```
```lua
mount ADDRESS PATH
```
```lua
mount --bind PATH PATH
```

## 用途

不带参数时，输出当前已挂载的文件系统列表。给出设备和目标路径时，会把对应文件系统挂接到统一的 OpenOS 目录树中；使用 `--bind` 时则创建目录到目录的绑定挂载。

## 参数

- `LABEL`: 要挂载的文件系统标签。
- `ADDRESS`: 要挂载的文件系统地址前缀。
- `PATH`: OpenOS 目录树中的挂载点路径。

## 选项

- `-r, --readonly`: 以只读方式挂载文件系统。
- `--bind`: 创建目录到目录的绑定挂载。
- `-h, --help`: 输出帮助用法信息。

## 示例

```lua
mount
```
显示当前全部挂载点。

```lua
mount test /home
```
把标签为 `test` 的文件系统挂载到 `/home`。

```lua
mount 56f /var
```
把地址以 `56f` 开头的文件系统挂载到 `/var`。

```lua
mount --readonly tmpfs /tmp_ro
```
把 `tmpfs` 以只读方式挂载到 `/tmp_ro`。

```lua
mount --bind /mnt/fa4/home /home
```
把 `/mnt/fa4/home` 绑定挂载到 `/home`。

## 备注

- 源设备可以通过标签、地址前缀，或在使用 `--bind` 时通过目录路径来指定。
- 如果只给了选项但缺少必要的位置参数，命令会输出用法并以错误状态退出。
- 注意：地址可以使用缩写形式，只要能够唯一匹配。
