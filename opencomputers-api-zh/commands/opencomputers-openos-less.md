# less

## 概述

使用可滚动的缓冲分页器查看文件或管道文本。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\less.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\less`

## 语法

```lua
less [FILE]
```

## 用途

行为类似 `more`，但会保留缓冲区，因此你可以按行或按页向前、向后滚动浏览内容。

## 参数

- `FILE`: 可选，要打开的文件。省略时从标准输入读取。

## 示例

```lua
less example.txt
```
在分页器中显示 `example.txt` 的内容。

```lua
find / | less
```
把命令输出通过管道送入 `less`，以便交互式滚动查看。

## 备注

- 随附手册列出的按键包括 `q`、上下方向键、空格、PageDown、PageUp、Home 和 End。
