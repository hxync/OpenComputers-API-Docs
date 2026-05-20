# internet

## 概述

通过主网络卡发起 HTTP 请求并打开带缓冲的套接字流。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\internet.lua`

## 用法

```lua
local internet = require("internet")
```

## 接口

### internet.open(address, port)

- 说明: 连接到某个主机或 `host:port` 组合，并把套接字包装成带缓冲的可读写流。
- 参数:
  - `address`
  - `port`

```lua
internet.open("address", nil)
```

### internet.request(url, data, headers, method)

- 说明: 发起一个 HTTP 请求，并返回一个可迭代读取响应分块的对象。
- 参数:
  - `url`
  - `data`
  - `headers`
  - `method`

```lua
internet.request(nil, nil, nil, nil)
```

### internet.socket(address, port)

- 说明: 打开一个由主网络卡支持的原始套接字式流对象。
- 参数:
  - `address`
  - `port`

```lua
internet.socket("address", nil)
```

## 备注

- 本页只列出模块导出的公开函数。
