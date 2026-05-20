# head

## 概述

输出文件或标准输入开头的若干行或若干字节。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\head.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\head`

## 语法

```lua
head [OPTION]... [FILE]...
```

## 用途

读取每个输入源的起始部分。默认情况下 `head` 会输出前 `10` 行，但也可以切换为按字节计数、在给出前导负号时输出“除了最后 `n` 项之外”的内容，并控制是否显示文件头。

## 参数

- `FILE...`: 要读取的文件。使用 `-` 或完全省略文件参数时会改为读取标准输入。

## 选项

- `--bytes=[-]n`: 输出前 `n` 个字节；若 `n` 以前导 `-` 给出，则输出除最后 `n` 个字节之外的全部内容。
- `--lines=[-]n`: 输出前 `n` 行；若 `n` 以前导 `-` 给出，则输出除最后 `n` 行之外的全部内容。
- `-q, --quiet, --silent`: 读取多个文件时永远不输出文件名头部。
- `-v, --verbose`: 始终输出文件名头部。
- `--help`: 输出帮助信息后退出。

## 示例

```lua
head -
```
从标准输入读取前 10 行，然后停止。

```lua
head file.txt
```
输出 `file.txt` 的前 10 行。

```lua
head --lines=32 file.txt
```
输出 `file.txt` 的前 32 行。
