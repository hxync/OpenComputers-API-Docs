# libarmor

## 概述

把一个表保护在代理后面，保留读取和迭代能力，同时拒绝未授权写入。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\libarmor.lua`

## 用法

```lua
local libarmor = require("libarmor")
```

## 接口

### libarmor.protect(original, whitelist)

- 说明: 围绕原始表返回一个受保护代理，并且只允许对白名单中出现的键进行写入。
- 参数:
  - `original`: 要通过代理暴露其可读内容的原始表。
  - `whitelist`: 可选，定义哪些键允许通过代理重新赋值的表。

```lua
local safe = libarmor.protect(config, {volume=true})
```

## 备注

- 本页只列出模块导出的公开函数。
