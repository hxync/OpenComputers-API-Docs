# modem

## 概述

本页记录的是 OpenSecurity 安全网络卡额外增加的那个回调。虽然生成出来的文件名是 `opensecurity-secure-network-card-driver.md`，但这张卡在 Lua 中真正暴露的组件名仍然是 `modem`。

由于这个驱动直接继承了 OpenComputers 标准网卡实现，所以普通的 `modem` / network card API 仍然全部可用。OpenSecurity 只额外加入了一个回调：`generateUUID()`。

## 可用性

- 仓库：`opensecurity`
- 实际组件名：`modem`
- 常见宿主：OpenSecurity 安全网络卡

## 用法

```lua
local component = require("component")
local modem = component.modem
```

如果当前机器里装有多个调制解调器类设备，建议使用 `component.proxy(address)` 明确指定这张安全网络卡。

## 扩展接口

### generateUUID

- 语法：`modem.generateUUID()`
- 返回：`true`
- 用途：重建调制解调器节点，使其获得一个新的网络地址。

行为说明：

- 当前节点地址会被丢弃。
- 驱动会创建一个新的 `modem` 节点，并得到新的 UUID 或地址。
- 然后它会保存旧邻居列表、加入新的 OpenComputers 网络，再把原邻居重新连接回去。
- 因此，如果你的程序缓存了旧组件地址，那么调用之后必须重新发现这张卡。

示例：

```lua
print("Old address:", modem.address)
assert(modem.generateUUID())
```

## 说明

- 这个回调没有显式定义失败返回值。
- 除了“重新随机化地址”这一能力之外，这张卡的其他行为仍然与普通 `modem` 一样。

## 相关内容

- `component`
- `opencomputers-network-card`
