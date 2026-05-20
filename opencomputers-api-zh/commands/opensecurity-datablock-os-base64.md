# os_base64

## 概述

使用 DataBlock 辅助库进行 Base64 编码或解码。

## 可用性

- 仓库: `opensecurity`
- 系统: `datablock`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_base64.lua`

## 语法

```lua
os_base64 [FILE...]
```
```lua
os_base64 -d [FILE...]
```
```lua
os_base64 --decode [FILE...]
```

## 用途

对标准输入或提供的文件使用 `datablock.encode64` 或 `datablock.decode64`。编码时按 3 字节分块读取，解码时按 4 个 Base64 字符分块读取。

## 参数

- `FILE...`: 可选，要编码或解码的文件。省略时从标准输入读取。

## 选项

- `-d, --decode`: 执行 Base64 解码，而不是编码。
- `-h, --help`: 输出一条指向 `base64` 手册页的简短提示。

## 示例

```lua
os_base64 file.bin
```
对 `file.bin` 进行编码，并把 Base64 结果写到标准输出。

```lua
os_base64 -d payload.txt
```
解码 `payload.txt` 中的 Base64 文本。
