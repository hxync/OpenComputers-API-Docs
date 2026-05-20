# uuid

## 概述

一个小型的 UUID v4 风格生成器，会把随机字节格式化为规范的十六进制分组字符串。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\uuid.lua`

## 用法

```lua
local uuid = require("uuid")
```

## 接口

### uuid.next()

- 说明: 生成一个随机的 RFC 4122 第 4 版风格 UUID 字符串，例如 `3c44c8a9-0613-46a2-ad33-97b6ba2e9d9a`。

```lua
local id = uuid.next()
```

## 备注

- 本页只列出模块导出的公开函数。
