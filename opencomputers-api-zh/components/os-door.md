# os_door

## 概述

`os_door` 组件用于控制 OpenSecurity 的门控制器，并通过它管理相邻的安全门。它可以查询门当前是否打开、切换开关状态，以及设置、修改、移除安全门内部保存的密码。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_door`
- 常见宿主：放置在世界中的 OpenSecurity 门控制器

## 用法

```lua
local component = require("component")
local door = component.os_door

if not door then
  error("未安装 os_door 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定控制器。

## 控制器前提

- 控制器会在相邻方块中搜索 OpenSecurity 安全门。
- 控制器与它连接的安全门必须拥有相同的所有者 UUID。
- 每次真正切换门状态都会消耗 `5` 点缓冲能量。
- 如果门设置了密码，那么 `open`、`close`、`toggle` 都需要把密码作为第一个参数传入。

## 接口

### greet

- 语法：`door.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置问候语。

固定返回文本：

```text
Lasciate ogne speranza, voi ch'intrate
```

示例：

```lua
print(door.greet())
```

### isOpen

- 语法：`door.isOpen()`
- 返回：`boolean`
- 用途：返回当前连接的安全门是否处于开启状态。

示例：

```lua
print("门是否打开：", door.isOpen())
```

### open

- 语法：`door.open([password])`
- 返回：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：当门当前关闭时将其打开。

行为说明：

- 如果门本来就是开着的，直接返回 `true`。
- 如果门设置了密码，则第一个参数必须与当前密码匹配。
- 只有需要真正改变状态时，才会经过内部 `toggle` 逻辑并消耗能量。

示例：

```lua
local ok, reason = door.open("vault123")
if not ok then
  error(reason)
end
```

### close

- 语法：`door.close([password])`
- 返回：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：当门当前打开时将其关闭。

行为说明：

- 如果门已经关闭，直接返回 `true`。
- 如果门设置了密码，则第一个参数必须与当前密码匹配。

示例：

```lua
local ok, reason = door.close("vault123")
if not ok then
  error(reason)
end
```

### toggle

- 语法：`door.toggle([password])`
- 返回：
  - 成功：`boolean`
  - 失败：`false, reason`
- 用途：在开启和关闭之间切换连接的安全门。

行为说明：

- 每次调用会消耗 `5` 点缓冲能量。
- 成功时返回切换后的新开门状态。

常见失败结果：

- `false, "Password Incorrect"`
- `false, "Owner of Controller and Door do not match."`
- `false, "Not enough power in OC Network."`

示例：

```lua
local openNow, reason = door.toggle("vault123")
if openNow == false and reason then
  error(reason)
end
print("切换后的门状态：", openNow)
```

### setPassword

- 语法：
  - `door.setPassword(newPassword)`
  - `door.setPassword(oldPassword, newPassword)`
- 返回：`boolean, message`
- 用途：设置或修改连接安全门中保存的密码。

行为说明：

- 如果门当前没有密码，只传入一个新密码即可。
- 如果门已经有密码，则第一个参数传旧密码，第二个参数传新密码。

常见返回值：

- `true, "Password set"`
- `true, "Password Changed"`
- `false, "Password was not changed"`
- `false, "Owner of Controller and Door do not match."`

示例：

```lua
assert(select(1, door.setPassword("vault123")))
```

### removePassword

- 语法：`door.removePassword(currentPassword)`
- 返回：`boolean, message`
- 用途：清除连接安全门当前设置的密码。

行为说明：

- 提供的密码必须与门中保存的密码一致。
- 控制器和门的所有者必须一致。

常见返回值：

- `true, "Password Removed"`
- `false, "Password was not removed"`
- `false, "Owner of Controller and Door do not match."`

示例：

```lua
local ok, message = door.removePassword("vault123")
print(ok, message)
```

## 示例

```lua
local password = "vault123"

if not door.isOpen() then
  local ok, reason = door.open(password)
  if not ok then
    error(reason)
  end
end

os.sleep(1)
door.close(password)
```

## 相关内容

- `component`
- `os_biometric`
