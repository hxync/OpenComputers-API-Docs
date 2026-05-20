# os_inflate

## 概述

使用 DataBlock 的 inflate 例程解压数据。

## 可用性

- 仓库: `opensecurity`
- 系统: `datablock`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_inflate.lua`

## 语法

```lua
os_inflate
```
```lua
os_inflate FILE
```

## 用途

从标准输入或第一个文件参数读取全部数据，然后把 inflate 解压结果写到标准输出。

## 参数

- `FILE`: 可选，要解压的文件。省略时从标准输入读取。

## 示例

```lua
os_inflate archive.deflated > archive.txt
```
解压 `archive.deflated`，并把恢复后的数据重定向保存。

## 备注

- 这个实现会先把完整输入加载到内存里，再执行解压。
