# rm

## 概述

删除文件、符号链接或目录。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\rm.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\rm`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\rm`

## 语法

```lua
rm [OPTIONS] FILE...
```

## 用途

按顺序逐个删除给定路径。普通文件和链接可以直接删除；空目录需要 `-d`；非空目录则必须启用递归删除。

## 参数

- `FILE...`: 一个或多个要删除的文件、链接或目录。

## 选项

- `-f, --force`: 忽略不存在的文件，关闭交互提示，并抑制常规状态输出。
- `-i`: 每次删除前都提示确认。
- `-I`: 当要删除的参数超过三个时，先统一提示一次确认。
- `-r, -R, --recursive`: 递归删除目录及其内容。
- `-d, --dir`: 允许在不启用递归模式时删除空目录。
- `-v, --verbose`: 每成功删除一个路径就输出一条消息。
- `--help`: 输出内置帮助文本。

## 示例

```lua
rm a.txt
```
删除文件 `a.txt`。

```lua
rm -d empty_dir
```
如果 `empty_dir` 为空目录，则将其删除。

```lua
rm -r cache
```
递归删除 `cache` 目录及其中的全部内容。

```lua
rm -iv a.txt b.txt
```
逐项提示确认，并输出每一次成功删除的结果。

## 备注

- 只读目标即使在递归模式下也会被拒绝删除。
- 对于已挂载的文件系统，请使用 `umount`，不要尝试用 `rm` 删除挂载路径。
