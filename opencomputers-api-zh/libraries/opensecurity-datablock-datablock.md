# datablock

## 概述

OpenSecurity 的数据块辅助库，可通过数据块组件完成哈希、Base64、ROT13、DEFLATE 和十六进制转换。

## 可用性

- 仓库: `opensecurity`
- 系统: `datablock`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\lib\datablock.lua`

## 用法

```lua
local datablock = require("datablock")
```

## 接口

### datablock.crc32(data)

- 说明: 返回给定数据的原始二进制 CRC-32 摘要。
- 参数:
  - `data`

```lua
datablock.crc32(nil)
```

### datablock.decode64(data)

- 说明: 通过数据块组件解码一个 Base64 字符串。
- 参数:
  - `data`

```lua
datablock.decode64(nil)
```

### datablock.deflate(data)

- 说明: 通过数据块组件使用 DEFLATE 算法压缩一个字符串。
- 参数:
  - `data`

```lua
datablock.deflate(nil)
```

### datablock.encode64(data)

- 说明: 通过数据块组件把一个字符串编码为 Base64。
- 参数:
  - `data`

```lua
datablock.encode64(nil)
```

### datablock.fromHex(hex)

- 说明: 把十六进制文本转换回原始二进制数据。
- 参数:
  - `hex`

```lua
datablock.fromHex(nil)
```

### datablock.inflate(data)

- 说明: 通过数据块组件解压一个经过 DEFLATE 压缩的字符串。
- 参数:
  - `data`

```lua
datablock.inflate(nil)
```

### datablock.md5(data)

- 说明: 返回给定数据的原始二进制 MD5 摘要。
- 参数:
  - `data`

```lua
datablock.md5(nil)
```

### datablock.rot13(data)

- 说明: 对一个字符串执行 ROT13 字母替换变换。
- 参数:
  - `data`

```lua
datablock.rot13(nil)
```

### datablock.sha256(data)

- 说明: 返回给定数据的原始二进制 SHA-256 摘要。
- 参数:
  - `data`

```lua
datablock.sha256(nil)
```

### datablock.toHex(data)

- 说明: 把原始二进制数据转换成大写十六进制文本。
- 参数:
  - `data`

```lua
datablock.toHex(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
