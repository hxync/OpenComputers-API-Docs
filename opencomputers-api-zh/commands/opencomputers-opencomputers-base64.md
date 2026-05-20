# base64

## 概述

对标准输入或文件内容进行 Base64 编码或解码。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\data\usr\bin\base64.lua`

## 语法

```lua
base64 [FILE...]
```
```lua
base64 -d [FILE...]
```
```lua
base64 --decode [FILE...]
```

## 用途

使用数据卡提供的 `data.encode64` 或 `data.decode64` 函数转换输入数据。如果没有提供文件参数，命令会从标准输入读取内容。

## 参数

- `FILE...`: 可选，要编码或解码的文件。省略时从标准输入读取。

## 选项

- `-d, --decode`: 执行 Base64 解码，而不是编码。
- `-h, --help`: 输出一条指向手册页的简短提示。

## 示例

```lua
base64 file.bin
```
把 `file.bin` 编码为 Base64，并把结果写到标准输出。

```lua
base64 -d payload.txt
```
解码 `payload.txt` 中的 Base64 内容，并把原始字节写到标准输出。

## 备注

- 编码时按 3 字节分块读取，解码时按 4 个 Base64 字符分块读取。
