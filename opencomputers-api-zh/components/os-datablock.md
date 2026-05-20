# os_datablock

## 概述

`os_datablock` 组件是 OpenSecurity 提供的数据处理方块。它内置了一组本地数据处理回调，例如 Base64、压缩/解压、哈希、ROT13 和 BCrypt 工具，同时还会挂载一个名为 `datablock` 的内置文件系统环境。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_datablock`
- 常见宿主：放置在世界中的 OpenSecurity 数据块

## 用法

```lua
local component = require("component")
local data = component.os_datablock

if not data then
  error("未安装 os_datablock 组件")
end
```

## 文件系统环境

当它连接到某台电脑上下文时，该方块还会额外挂载一个名为 `datablock` 的内置文件系统环境。

## 数据限制与能量

有一部分字节数组类回调会共用同一套限制器：

- 硬上限：`Settings.get().dataCardHardLimit()`
- 软上限：`Settings.get().dataCardSoftLimit()`
- 当数据超过软上限但仍在允许范围内时，调用会暂停 `Settings.get().dataCardTimeout()`

这些受限回调的共同失败模式：

- 输入过大：`data size limit exceeded`
- 能量不足：`not enough energy`

源码中使用的能量分组：

- complex 成本回调：
  - `encode64`
  - `decode64`
  - `deflate`
  - `inflate`
- simple 成本回调：
  - `sha256`
  - `md5`
  - `crc32`
- 不经过限制器、也没有显式能耗判断的回调：
  - `rot13`
  - `bCryptHash`
  - `bCryptCheck`

## 接口

### greet

- 语法：`data.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置问候语。

### getLimit

- 语法：`data.getLimit()`
- 返回：`number`
- 用途：返回所有受限制回调允许接受的硬字节上限。

### encode64

- 语法：`data.encode64(input)`
- 返回：`string`
- 用途：对输入字节串进行 Base64 编码。

参数说明：

- `input:string|byte[]`

行为说明：

- 返回值是包含 ASCII Base64 文本的字节字符串。

### decode64

- 语法：`data.decode64(input)`
- 返回：`string`
- 用途：对输入字节串进行 Base64 解码。

参数说明：

- `input:string|byte[]`

行为说明：

- 返回值是解码后的原始字节字符串。

### deflate

- 语法：`data.deflate(input)`
- 返回：`string`
- 用途：使用 deflate 算法压缩输入字节串。

参数说明：

- `input:string|byte[]`

### inflate

- 语法：`data.inflate(input)`
- 返回：`string`
- 用途：对已 deflate 的字节串进行解压。

参数说明：

- `input:string|byte[]`

### sha256

- 语法：`data.sha256(input)`
- 返回：`string`
- 用途：计算 SHA-256 摘要。

参数说明：

- `input:string|byte[]`

行为说明：

- 返回结果是二进制摘要，而不是十六进制文本。

### md5

- 语法：`data.md5(input)`
- 返回：`string`
- 用途：计算 MD5 摘要。

参数说明：

- `input:string|byte[]`

行为说明：

- 返回结果是二进制摘要。

### crc32

- 语法：`data.crc32(input)`
- 返回：`string`
- 用途：计算 CRC-32 摘要。

参数说明：

- `input:string|byte[]`

行为说明：

- 返回结果是二进制摘要。

### rot13

- 语法：`data.rot13(input)`
- 返回：`string`
- 用途：对 ASCII 字母应用 ROT13 变换。

参数说明：

- `input:string`

行为说明：

- 只有 `A-Z` 和 `a-z` 会被轮转。
- 其他字符保持不变。

### bCryptHash

- 语法：`data.bCryptHash(plaintext[, rounds])`
- 返回：`string`
- 用途：计算 BCrypt 哈希字符串。

参数说明：

- `plaintext:string`
- `rounds:number` 可选

行为说明：

- 默认 rounds 值为 `10`。
- 实现会把 rounds 限制在 `4` 到 `15`。
- 返回值是标准的 BCrypt 文本哈希。

### bCryptCheck

- 语法：`data.bCryptCheck(plaintext, hash)`
- 返回：`boolean`
- 用途：用明文校验一个 BCrypt 哈希字符串。

参数说明：

- `plaintext:string`
- `hash:string`

## 示例

```lua
local digest = data.sha256("hello")
local encoded = data.encode64("secret")
local compressed = data.deflate("compress me")
local hash = data.bCryptHash("password", 12)

print(#digest, encoded, data.bCryptCheck("password", hash))
print(data.rot13("uryyb jbeyq"))
```

## 相关内容

- `component`
- `os_cardwriter`
