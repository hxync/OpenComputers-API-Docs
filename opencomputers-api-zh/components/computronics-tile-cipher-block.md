# cipher

## 概述

`cipher` 是 Computronics 提供的基础加密方块组件。它会根据方块内部 6 个物品槽中的物品内容推导出 AES 密钥和初始化向量，再用这些推导结果对数据进行加密或解密。

这个方块适合做“实体物品就是密钥”的场景。只要插入物品发生变化，最终的加密结果也会随之改变。

## 可用性

- 仓库：`computronics`
- 组件名：`cipher`
- 常见宿主：放置在世界中的 Computronics 加密方块

## 用法

```lua
local component = require("component")
local cipher = component.cipher

if not cipher then
  error("未安装 cipher 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 密钥模型

- 该方块把内部 6 个物品槽当作实体密钥材料。
- 物品 id、堆叠数量、损伤值和 NBT 都会参与最终密钥推导。
- 最终使用的算法是 `AES/CBC/PKCS5Padding`。
- 方块会推导出：
  - 一个 16 字节 AES 密钥
  - 一个 16 字节初始化向量

只要任意槽位内容改变，最终推导出的密钥就会立刻变化。

## 锁定机制

该方块可以被锁定。锁定后：

- 普通物品栏访问无法读取槽位内容
- 不能插入或取出物品
- 侧面物品栏访问会报告“没有可访问槽位”

在配置允许加密块锁定时，这个锁定状态也会被保存到 NBT 中。

## 接口

### encrypt

- 语法：`cipher.encrypt(message)`
- 返回值：`string`
- 用途：使用当前由物品推导出的 AES 密钥来加密数据。

参数说明：

- `message:string|byte[]`：要加密的明文。

行为说明：

- 传入 Lua 字符串时，会先按 UTF-8 编码。
- 返回结果是 Base64 字符串。
- 如果底层 Java cipher 实例初始化失败，实现会返回空字符串。

示例：

```lua
local encoded = cipher.encrypt("secret message")
print(encoded)
```

### decrypt

- 语法：`cipher.decrypt(message)`
- 返回值：`string`
- 用途：使用当前推导出的 AES 密钥来解密 Base64 密文。

参数说明：

- `message:string`：由 `cipher.encrypt` 生成的 Base64 密文。

行为说明：

- 解密后的字节会按 UTF-8 字符串返回。
- 如果密文来自不同的物品布局，通常会直接抛出解密错误。

示例：

```lua
local encoded = cipher.encrypt("hello")
local decoded = cipher.decrypt(encoded)
print(decoded)
```

### isLocked

- 语法：`cipher.isLocked()`
- 返回值：`boolean`
- 用途：返回当前方块是否处于锁定状态。

示例：

```lua
print("当前是否锁定:", cipher.isLocked())
```

### setLocked

- 语法：`cipher.setLocked(locked)`
- 返回值：没有可依赖的有效返回值
- 用途：开启或关闭方块的锁定状态。

参数说明：

- `locked:boolean`：`true` 表示锁定，`false` 表示解锁。

实际使用时应把它当成纯 setter，看作没有实用返回值即可。

示例：

```lua
cipher.setLocked(true)
```

## 示例

```lua
local plaintext = "door access token"
local ciphertext = cipher.encrypt(plaintext)

print("Encrypted:", ciphertext)
print("Decrypted:", cipher.decrypt(ciphertext))
```

## 说明

- 由于密钥来自物品栏内容，移动或替换已插入物品后，旧密文可能就再也无法解开。
- 这是基础的对称加密方块；如果需要显式 RSA 密钥处理，请使用高级加密方块。

## 相关内容

- `component`
- `advanced_cipher`
