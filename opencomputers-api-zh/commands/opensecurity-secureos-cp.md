# cp

## 概述

复制文件、符号链接或整个目录树。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\cp.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\cp`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\cp`

## 语法

```lua
cp [OPTIONS] SOURCE... DEST
```

## 用途

把一个或多个源路径复制到目标文件或目标目录。普通文件通过 `filesystem.copy` 复制；目录必须配合 `-r`；符号链接则可以选择按链接本身保留。

## 参数

- `SOURCE...`: 一个或多个要复制的源文件、目录或链接。
- `DEST`: 目标文件路径，或接收结果的目标目录。

## 选项

- `-i`: 覆盖已存在的目标文件前先提示确认。这个选项优先于 `-n`。
- `-n`: 如果目标文件已存在，则不覆盖。
- `-r`: 递归复制目录。
- `-u`: 只有在目标文件不存在，或内容与源文件不同时才复制。
- `-P`: 保留符号链接本身，而不是复制它所指向的目标内容。
- `-v`: 在执行复制时输出每一步操作。
- `-x`: 递归复制目录时停留在源文件系统内，跳过挂载到其下方的其他文件系统。
- `--skip=PATTERN`: 跳过解析后完整路径匹配该 Lua 模式的源路径。

## 示例

```lua
cp a.txt b.txt
```
把 `a.txt` 复制成新的 `b.txt`。

```lua
cp a.txt b.txt /tmp
```
把 `a.txt` 和 `b.txt` 一起复制到 `/tmp` 目录。

```lua
cp -r /home/project /backup
```
把 `/home/project` 整个目录树递归复制到 `/backup`。

```lua
cp -n /etc/hostname /tmp/hostname
```
只有当 `/tmp/hostname` 还不存在时才复制该文件。

## 备注

- 如果源路径以 `.` 或 `..` 结尾，命令会复制该目录的内容，而不是把目录本身再嵌套一层。
- 目标文件系统必须可写，否则命令会在传输开始前直接失败。
