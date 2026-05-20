# advanced_cipher

## 概述

`advanced_cipher` 是 Computronics 提供的 RSA 加密方块组件。它可以生成 RSA 密钥对、使用公钥加密、使用私钥解密，并返回一个异步密钥生成句柄。

如果你需要显式密钥交换，而不是依赖物品栏推导出的对称密钥，就应该使用这个方块。

## 可用性

- 仓库：`computronics`
- 组件名：`advanced_cipher`
- 常见宿主：放置在世界中的 Computronics 高级加密方块

## 用法

```lua
local component = require("component")
local advanced = component.advanced_cipher

if not advanced then
  error("未安装 advanced_cipher 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 密钥表结构

RSA 密钥在 Lua 里用整数键表表示：

- `key[1]`：Base64 编码后的模数 `n`
- `key[2]`：Base64 编码后的指数
- `key[3]`：可选字符串 `"prime"`

这里有两种密钥风格：

- 由 Java 标准 RSA 密钥对生成的普通 RSA 密钥
- 由显式素数生成的 prime 模式密钥，带 `key[3] == "prime"` 标记

高级加密块既接受整数键 `1`、`2`、`3`，也接受 Lua 中常见的数字键 `1.0`、`2.0`、`3.0`。

## 能量模型

- 内部能量存储默认是 `1600`
- 生成密钥会消耗配置中的密钥生成能量
- 加密与解密会消耗：
  - 一份基础工作能量
  - 再加上 `0.2 * 消息长度`

如果能量不足，OpenComputers 回调会返回 `nil, nil, reason`。

## 接口

### createRandomKeySet

- 语法：`advanced.createRandomKeySet([keyLength])`
- 返回值：`keygen`
- 用途：启动一次异步 RSA 密钥生成，并返回 RSA 生成器句柄。

参数说明：

- `keyLength:number` 可选：请求的 RSA 位数。

行为说明：

- 省略时默认生成 `1024` 位密钥。
- 合法位数范围是 `1` 到 `2048`。
- OpenComputers 上下文在启动任务后会暂停 `0.5` 秒。

错误情况：

- 非法位数会抛出 `bitlength must be between 1 and 2048`

示例：

```lua
local keygen = advanced.createRandomKeySet(1024)
```

### createKeySet

- 语法：`advanced.createKeySet(num1, num2)`
- 返回值：`keygen`
- 用途：使用两个显式给定的素数启动一次异步 RSA 密钥生成。

参数说明：

- `num1:number`：第一个素数
- `num2:number`：第二个素数

行为说明：

- 两个数字都必须是素数。
- 这样生成出来的密钥会被标记为 prime 模式密钥。

错误情况：

- 不是素数时会抛出类似：
  - `bad argument #0 (prime expected, got X)`
  - `bad argument #1 (prime expected, got X)`

示例：

```lua
local keygen = advanced.createKeySet(61, 53)
```

### encrypt

- 语法：`advanced.encrypt(message, publicKey)`
- 返回值：
  - 成功：`ciphertext`
  - 失败：`nil, reason`
- 用途：使用给定 RSA 公钥对字节串进行加密。

参数说明：

- `message:string|byte[]`：明文
- `publicKey:table`：上文描述的密钥表

行为说明：

- 在 OpenComputers 回调里，消息会按字节数组处理。
- 返回值是 Base64 编码的密文。
- prime 模式密钥会直接使用模幂运算。
- 普通密钥会使用 Java 标准 RSA 加密。

常见失败：

- `nil, "an error occured during encryption"`
- 密钥过小导致无法容纳消息的错误
- 能量不足时返回 `nil, nil, reason`

示例：

```lua
local publicKey, privateKey
local keygen = advanced.createRandomKeySet(1024)
while not keygen.finished() do
  os.sleep(0.1)
end
publicKey, privateKey = keygen.getKeys()

local ciphertext = assert(advanced.encrypt("hello", publicKey))
```

### decrypt

- 语法：`advanced.decrypt(message, privateKey)`
- 返回值：
  - 成功：`plaintext`
  - 失败：`nil, reason`
- 用途：使用给定私钥解密 Base64 编码的 RSA 密文。

参数说明：

- `message:string|byte[]`：Base64 密文
- `privateKey:table`：上文描述的密钥表

行为说明：

- prime 模式密钥会直接使用模幂运算。
- 普通密钥会使用 Java 标准 RSA 解密。
- 最终结果按 UTF-8 字符串返回。

常见失败：

- `nil, "an error occured during decryption"`
- 密钥过小错误
- 能量不足时返回 `nil, nil, reason`

示例：

```lua
local decoded = assert(advanced.decrypt(ciphertext, privateKey))
print(decoded)
```

## Keygen 句柄

`createRandomKeySet` 和 `createKeySet` 返回的都是一个类似 userdata 的 RSA 生成器对象。这个对象额外提供：

- `finished()`
- `getKeys()`

完整行为请参考单独的 `computronics-rsavalue` 页面。

## 相关内容

- `component`
- `cipher`
- `computronics-rsavalue`
