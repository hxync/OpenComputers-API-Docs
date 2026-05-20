# sha

## 概述

SecureOS 的 SHA-256 辅助库：可在纯 Lua 中运行，也可在可用时委托给主数据卡组件。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\sha256.lua`

## 用法

```lua
local sha = require("sha256")
```

## 接口

### sha.sha256(msg)

- 说明: 返回一个二进制字符串的十六进制 SHA-256 摘要；若可用则优先使用数据卡实现。
- 参数:
  - `msg`: 要计算哈希的二进制字符串。

```lua
local digest = sha.sha256('hello')
```

## 备注

- 本页只列出模块导出的公开函数。
