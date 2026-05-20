# md5sum

## 概述

为标准输入或一个或多个文件计算 MD5 摘要。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\data\usr\bin\md5sum.lua`

## 语法

```lua
md5sum [FILE...]
```

## 用途

把完整输入读入内存，计算 MD5 摘要，并以十六进制文本形式输出结果。如果提供了文件参数，则会为每个文件分别输出一行摘要。

## 参数

- `FILE...`: 可选，要计算摘要的文件。省略时从标准输入读取全部数据。

## 示例

```lua
md5sum file.bin
```
输出 `file.bin` 的 MD5 摘要，并在后面附带文件名。

```lua
cat file.bin | md5sum
```
为管道输入的数据计算 MD5 摘要。

## 备注

- 这个实现会在计算之前先把整个文件完整读入内存。
