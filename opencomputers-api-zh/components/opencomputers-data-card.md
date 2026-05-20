# opencomputers-data-card

## 概述

这个文件名是历史遗留名称，真实的 Lua 组件名是 `data`。

数据卡组件提供哈希、编码、压缩、加密、安全随机数以及椭圆曲线密钥运算功能。不同等级的数据卡可用接口不同。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`data`
- 对应实现：`li.cil.oc.server.component.DataCard`
- 常见宿主：安装了数据卡的电脑、服务器或其他设备

## 等级划分

### T1

- `getLimit`
- `encode64`
- `decode64`
- `deflate`
- `inflate`
- `crc32`
- `md5`
- `sha256`
- `decodeNBT`

### T2

额外增加：

- `md5` 的 HMAC 用法
- `sha256` 的 HMAC 用法
- `encrypt`
- `decrypt`
- `random`

### T3

额外增加：

- `generateKeyPair`
- `deserializeKey`
- `ecdh`
- `ecdsa`

## 重要规则

### 二进制字符串

很多接口返回的是原始二进制数据，不是可直接阅读的文本。

典型例子包括：

- 哈希结果
- AES 密文
- 随机字节
- 序列化后的密钥
- ECDSA 签名

如果你要把这些结果输出、保存或通过文本协议传输，通常应该先用 `encode64()` 做 Base64 编码。

### 数据长度限制

`getLimit()` 返回数据卡字节处理接口允许接收的硬上限。

- 如果第一个字符串或字节参数超过这个限制，会直接抛出 `data size limit exceeded`。
- 如果输入虽然没超过硬上限，但超过软上限，当前计算机会额外暂停一段配置好的时间。

### 能量消耗

所有接口都会消耗连接器缓冲能量。

- 如果宿主能量不足，接口会失败并报出 `not enough energy`。

## data API

### getLimit

- 语法：`data.getLimit()`
- 返回：`number`
- 用途：返回数据卡字节处理接口允许接受的最大字节数。

### encode64

- 语法：`data.encode64(bytes)`
- 参数：
  - `bytes:string`：原始二进制或普通文本。
- 返回：`string`
- 用途：对输入数据执行 Base64 编码。

### decode64

- 语法：`data.decode64(text)`
- 参数：
  - `text:string`：Base64 文本。
- 返回：`string`
- 用途：把 Base64 文本解码回原始字节。

### deflate

- 语法：`data.deflate(bytes)`
- 参数：
  - `bytes:string`：原始输入数据。
- 返回：`string`
- 用途：使用 deflate 格式压缩数据。

### inflate

- 语法：`data.inflate(bytes)`
- 参数：
  - `bytes:string`：deflate 压缩后的二进制数据。
- 返回：`string`
- 用途：解压 deflate 格式数据。

### crc32

- 语法：`data.crc32(bytes)`
- 参数：
  - `bytes:string`
- 返回：`string`
- 用途：计算输入数据的 CRC-32 摘要。

返回值是 4 字节的二进制摘要，不是十六进制文本。

### md5

- 语法：
  - `data.md5(bytes)`
  - `data.md5(bytes, hmacKey)`
- 参数：
  - `bytes:string`
  - `hmacKey:string`：可选，HMAC 密钥，仅 T2 及以上支持。
- 返回：`string`
- 用途：计算普通 MD5 或 HMAC-MD5 摘要。

说明：

- 普通 MD5 在 T1 就可用。
- HMAC 形式需要 T2 或更高等级。
- 返回值是二进制数据。

### sha256

- 语法：
  - `data.sha256(bytes)`
  - `data.sha256(bytes, hmacKey)`
- 参数：
  - `bytes:string`
  - `hmacKey:string`：可选，HMAC 密钥，仅 T2 及以上支持。
- 返回：`string`
- 用途：计算普通 SHA-256 或 HMAC-SHA256 摘要。

返回值是二进制数据。

### decodeNBT

- 语法：`data.decodeNBT(bytes)`
- 参数：
  - `bytes:string`：gzip 压缩过的二进制 NBT 数据。
- 返回：`table`
- 用途：把压缩的二进制 NBT 数据解码为 Lua 表结构。

这个接口很适合用来检查 Minecraft 的序列化数据块。

### encrypt

- 语法：`data.encrypt(bytes, key, iv)`
- 参数：
  - `bytes:string`：明文数据。
  - `key:string`：必须正好 16 字节。
  - `iv:string`：必须正好 16 字节。
- 返回：`string`
- 用途：使用 `AES/CBC/PKCS5Padding` 加密数据。

失败情况：

- 密钥长度错误：`expected a 128-bit AES key`
- IV 长度错误：`expected a 128-bit AES IV`

返回值是二进制密文。

### decrypt

- 语法：`data.decrypt(bytes, key, iv)`
- 参数：
  - `bytes:string`：密文。
  - `key:string`：必须正好 16 字节。
  - `iv:string`：必须正好 16 字节。
- 返回：`string`
- 用途：解密 `AES/CBC/PKCS5Padding` 格式的密文。

返回值是原始明文字节。

### random

- 语法：`data.random(length)`
- 参数：
  - `length:number`：要生成的随机字节数。
- 返回：`string`
- 用途：生成密码学安全的随机字节序列。

合法范围：

- `1` 到 `1024`

非法长度会抛出：

- `length must be in range [1..1024]`

### generateKeyPair

- 语法：`data.generateKeyPair([bitLen])`
- 参数：
  - `bitLen:number`：可选，只能是 `256` 或 `384`，默认 `384`。
- 返回：`userdata, userdata`
- 用途：生成一对椭圆曲线公钥和私钥。

返回顺序：

- 第一个返回值：公钥 `userdata`
- 第二个返回值：私钥 `userdata`

非法位数会抛出：

- `invalid key length, must be 256 or 384`

### deserializeKey

- 语法：`data.deserializeKey(bytes, typeName)`
- 参数：
  - `bytes:string`：序列化后的密钥字节。
  - `typeName:string`：`ec-public` 或 `ec-private`。
- 返回：`userdata`
- 用途：根据序列化字节恢复一个 EC 密钥对象。

非法类型名会抛出：

- `invalid key type, must be ec-public or ec-private`

### ecdh

- 语法：`data.ecdh(privateKey, publicKey)`
- 参数：
  - `privateKey:userdata`：EC 私钥对象。
  - `publicKey:userdata`：EC 公钥对象。
- 返回：`string`
- 用途：用 ECDH 派生共享密钥。

返回值是原始二进制共享密钥。

参数类型不匹配时会抛出类似错误：

- `private key expected at 1`
- `public key expected at 2`

### ecdsa

- 语法：
  - `data.ecdsa(bytes, privateKey)`
  - `data.ecdsa(bytes, publicKey, signature)`
- 参数：
  - `bytes:string`：消息字节。
  - `privateKey:userdata`：签名模式下使用。
  - `publicKey:userdata`：验签模式下使用。
  - `signature:string`：要验证的二进制签名。
- 返回：
  - 签名模式：`string`
  - 验签模式：`boolean`
- 用途：使用 `SHA256withECDSA` 对数据签名或验签。

行为说明：

- 不传 `signature` 时，执行签名并返回二进制签名。
- 传入 `signature` 时，执行验签并返回 `true` 或 `false`。

## EC 密钥 userdata API

`generateKeyPair()` 和 `deserializeKey()` 返回的密钥对象本身还带有以下方法。

### isPublic

- 语法：`key.isPublic()`
- 返回：`boolean`
- 用途：判断这个密钥对象是不是公钥。

### keyType

- 语法：`key.keyType()`
- 返回：`string`
- 用途：返回 `ec-public` 或 `ec-private`。

### serialize

- 语法：`key.serialize()`
- 返回：`string`
- 用途：返回该密钥的编码后二进制形式。

如果你要安全地保存或打印它，建议再用 `data.encode64()` 包一层。

## 示例

```lua
local component = require("component")
local data = component.data

local digest = data.sha256("hello world")
print("sha256(base64):", data.encode64(digest))

local publicKey, privateKey = data.generateKeyPair(256)
local signature = data.ecdsa("message", privateKey)
print("签名验证结果：", data.ecdsa("message", publicKey, signature))

local secret = data.random(16)
local iv = data.random(16)
local cipher = data.encrypt("secret text", secret, iv)
print("cipher(base64):", data.encode64(cipher))
print("plain:", data.decrypt(cipher, secret, iv))
```

## 相关内容

- `database`
- `internet`
