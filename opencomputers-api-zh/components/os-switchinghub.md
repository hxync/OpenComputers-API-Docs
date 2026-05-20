# os_switchinghub

## 概述

`os_switchinghub` 组件用于控制 OpenSecurity 的可切换集线器。它本质上是一个带方向控制的 OC 网络中继器，可以通过 Lua 单独开启或关闭各个面上的网络连接。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_switchinghub`
- 常见宿主：放置在世界中的 OpenSecurity 可切换集线器

## 用法

```lua
local component = require("component")
local hub = component.os_switchinghub

if not hub then
  error("未安装 os_switchinghub 组件")
end
```

## 网络行为

- 方块主朝向所在的一面始终会暴露主节点连接。
- 其他各个面可以分别单独启用或关闭。
- 每次修改某个面之后，节点都会先移除再重新加入 OC 网络，以便让新的网络拓扑立即生效。
- 每次成功切换一面都会消耗 `5` 点缓冲能量。

## 密码行为

- 密码是可选的。
- 如果当前还没有密码，`setPassword(newPassword)` 会直接写入初始密码。
- 如果已经存在密码，只有 `setPassword(oldPassword, newPassword)` 的第一个参数匹配时才会修改成功。
- `setSide` 只有在当前密码非空时才会校验密码参数。

## side 参数说明

`setSide(side, enabled[, password])` 使用的是 Forge 方向编号：

- `0`：down
- `1`：up
- `2`：north
- `3`：south
- `4`：west
- `5`：east

之后源码还会根据方块当前朝向，把这个本地方向重新映射成实际的物理面再执行启用或关闭。

## 接口

### greet

- 语法：`hub.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置问候语。

### setPassword

- 语法：
  - `hub.setPassword(newPassword)`
  - `hub.setPassword(oldPassword, newPassword)`
- 返回：`string`
- 用途：设置或修改集线器密码。

可能的返回字符串：

- `"Password set"`
- `"Password Changed"`
- `"Password was not changed"`

示例：

```lua
print(hub.setPassword("relay123"))
```

### setSide

- 语法：`hub.setSide(side, enabled[, password])`
- 返回：`string`
- 用途：启用或关闭一个经过朝向映射后的网络面。

参数说明：

- `side:number`：上表中的 Forge 方向编号。
- `enabled:boolean`：是否启用该面。
- `password:string` 可选：仅当当前已经设置密码时需要传入。

行为说明：

- 成功时返回 `"ok"`。
- 如果密码不匹配，则返回 `"Password Incorrect"`。
- 如果网络缓冲无法支付 `5` 点能量，则会抛出 `Not enough power in OC Network.`。

示例：

```lua
local result = hub.setSide(2, true, "relay123")
print(result)
```

## 示例

```lua
hub.setPassword("relay123")
hub.setSide(0, true, "relay123")
hub.setSide(1, true, "relay123")
hub.setSide(3, false, "relay123")
```

## 相关内容

- `component`
- `os_door`
