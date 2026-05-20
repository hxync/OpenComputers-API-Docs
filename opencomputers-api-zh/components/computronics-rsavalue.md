# computronics-rsavalue

## 概述

`computronics-rsavalue` 这一页描述的是高级 Computronics 加密方块返回的 RSA 密钥生成句柄。它不是一个直接安装在世界中的组件，而是以下接口返回的值对象：

- `advanced_cipher.createRandomKeySet(...)`
- `advanced_cipher.createKeySet(...)`

你需要通过这个句柄来查询密钥是否生成完成，以及在完成后取回最终密钥对。

## 可用性

- 仓库：`computronics`
- 值类型：RSA 密钥生成句柄
- 常见来源：`advanced_cipher` 的返回值

## 用法

```lua
local component = require("component")
local advanced = component.advanced_cipher
local keygen = advanced.createRandomKeySet(1024)
```

## 接口

### finished

- 语法：`keygen.finished()`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, reason`
- 用途：报告 RSA 密钥生成是否已经完成。

可能的失败返回：

- `nil, "calculation returned no key set"`
- `nil, "an error occured during key generation"`

示例：

```lua
while true do
  local done, reason = keygen.finished()
  if done == nil then
    error(reason)
  end
  if done then
    break
  end
  os.sleep(0.1)
end
```

### getKeys

- 语法：`keygen.getKeys()`
- 返回值：
  - 成功：`publicKey, privateKey`
  - 失败：`nil, nil, reason`
- 用途：在密钥生成完成后返回最终的公钥和私钥。

行为说明：

- 如果生成仍在进行，会返回：
  - `nil, nil, "key is still being generated"`
- 如果生成失败，会返回对应错误原因。

返回密钥格式：

- `publicKey[1]`：Base64 模数
- `publicKey[2]`：Base64 公钥指数
- `publicKey[3]`：可选 `"prime"`
- `privateKey[1]`：Base64 模数
- `privateKey[2]`：Base64 私钥指数
- `privateKey[3]`：可选 `"prime"`

示例：

```lua
local publicKey, privateKey, reason = keygen.getKeys()
if not publicKey then
  error(reason)
end

print("公钥模数:", publicKey[1])
```

## 示例

```lua
local keygen = component.advanced_cipher.createRandomKeySet(1024)

repeat
  os.sleep(0.1)
until keygen.finished()

local publicKey, privateKey = keygen.getKeys()
local encrypted = component.advanced_cipher.encrypt("hello", publicKey)
local decrypted = component.advanced_cipher.decrypt(encrypted, privateKey)
print(decrypted)
```

## 说明

- 这个值对象本身就是异步设计的。
- 如果在生成完成前丢弃这个句柄，你也会失去对这次待完成结果的直接访问能力。

## 相关内容

- `advanced_cipher`
