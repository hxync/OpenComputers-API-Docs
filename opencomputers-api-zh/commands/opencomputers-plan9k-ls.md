# ls

## 概述

列出文件和目录，并支持格式化、排序和递归显示。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\ls.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\ls`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\ls`

## 语法

```lua
ls [OPTIONS] [FILE...]
```

## 用途

显示指定文件或目录的信息；如果没有提供参数，则显示当前工作目录。命令支持长列表格式、递归遍历、多种排序方式、可选颜色输出，以及 Microsoft 风格的统计摘要。

## 参数

- `FILE...`: 可选，要查看的文件或目录。省略时列出当前工作目录。

## 选项

- `-a, --all`: 显示以 `.` 开头的隐藏条目。
- `--full-time`: 与 `-l` 配合时，以完整的近 ISO 日期时间格式输出时间戳。
- `-h, --human-readable`: 在输出大小时使用 1024 进制，并显示更易读的缩写单位。
- `--si`: 在输出大小时使用 1000 进制，而不是 1024。
- `-l`: 使用长列表格式，显示类型、可写标记、大小和修改时间。
- `-r, --reverse`: 反转最终输出顺序。
- `-R, --recursive`: 递归列出所有子目录。
- `-S`: 按文件大小排序，最大的排在前面。
- `-t`: 按修改时间排序，最新的排在前面。
- `-X`: 按文件扩展名字母顺序排序。
- `-1`: 每行只输出一个条目。
- `-p`: 在显示的目录名后追加 `/`。
- `-M`: 在每次列表后输出 Microsoft 风格的文件数、目录数和剩余空间统计。
- `--no-color`: 即使输出到终端，也禁用 ANSI 彩色显示。
- `--help`: 输出内置帮助文本。

## 示例

```lua
ls
```
列出当前工作目录。

```lua
ls -l /bin
```
以长列表格式显示 `/bin`。

```lua
ls -aR /home
```
递归列出 `/home`，并包含隐藏条目。

```lua
ls -M /mnt
```
列出 `/mnt`，并在后面输出 DOS 风格的文件和目录统计。

## 备注

- 当标准输出连接到终端时，普通列表会自动排成多列，除非使用了 `-1` 或 `-l`。
- 如果机器内存不足，无法加载完整实现，外层包装脚本会提示改用 `list` 命令。
