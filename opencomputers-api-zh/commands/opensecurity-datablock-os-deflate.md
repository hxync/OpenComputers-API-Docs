# os_deflate

## 概述

使用 DataBlock 的 deflate 例程压缩数据。

## 可用性

- 仓库: `opensecurity`
- 系统: `datablock`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_deflate.lua`

## 语法

```lua
os_deflate
```
```lua
os_deflate FILE
```

## 用途

从标准输入或第一个文件参数读取全部数据，然后把 deflate 压缩结果写到标准输出。

## 参数

- `FILE`: 可选，要压缩的文件。省略时从标准输入读取。

## 示例

```lua
os_deflate archive.txt > archive.deflated
```
压缩 `archive.txt`，并把压缩结果重定向保存。

## 备注

- 这个实现会先把完整输入加载到内存里，再执行压缩。
