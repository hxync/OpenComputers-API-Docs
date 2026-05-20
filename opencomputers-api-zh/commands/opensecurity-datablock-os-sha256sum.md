# os_sha256sum

## 概述

使用 DataBlock 辅助库计算 SHA-256 摘要。

## 可用性

- 仓库: `opensecurity`
- 系统: `datablock`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_sha256sum.lua`

## 语法

```lua
os_sha256sum [FILE...]
```

## 用途

完整读取标准输入或一个或多个文件，计算 SHA-256，并输出十六进制摘要文本。

## 参数

- `FILE...`: 可选，要计算摘要的文件。省略时从标准输入读取。

## 示例

```lua
os_sha256sum file.bin
```
输出 `file.bin` 的 SHA-256 摘要以及文件名。

## 备注

- 这个实现会在计算之前先把整个文件完整读入内存。
