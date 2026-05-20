# deflate

## 概述

使用数据卡的 deflate 例程压缩数据。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\deflate.lua`

## 语法

```lua
deflate
```
```lua
deflate FILE
```

## 用途

把标准输入或指定文件一次性完整读入内存，然后将 deflate 压缩后的结果写到标准输出。

## 参数

- `FILE`: 可选，要压缩的文件。省略时从标准输入读取。

## 示例

```lua
deflate document.txt > document.deflated
```
压缩 `document.txt`，并把压缩结果重定向到 `document.deflated`。

```lua
cat document.txt | deflate
```
压缩管道输入的数据流。

## 备注

- 这个实现会先把完整输入加载到内存里，再执行压缩。
