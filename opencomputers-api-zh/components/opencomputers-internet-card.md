# internet

## 概述

本页记录的是 OpenComputers 互联网卡组件。虽然生成出来的文件名是 `opencomputers-internet-card.md`，但它在 Lua 里真正暴露的组件名是 `internet`。

这个组件可以：

- 打开 TCP 套接字
- 发起 HTTP 请求
- 返回可轮询、可读写的连接句柄对象

## 可用性

- 仓库：`opencomputers`
- 实际组件名：`internet`
- 常见宿主：OpenComputers 互联网卡

## 用法

```lua
local component = require("component")
local internet = component.internet

if not internet then
  error("未安装 internet 组件")
end
```

## 所有权规则

- 这张卡会绑定到第一个与它建立邻接连接的电脑上下文。
- 之后所有回调调用都必须来自这个拥有者电脑。
- 否则实现会抛出 `can only be used by the owning computer`。
- 当拥有者电脑停止、启动或断开连接时，所有打开的互联网句柄都会被关闭。

## 配置开关限制

该组件还受 OpenComputers 配置控制：

- 完全禁止互联网访问：`internet access is unavailable`
- HTTP 被禁用：`http requests are unavailable`
- TCP 被禁用：`tcp connections are unavailable`
- 自定义 HTTP 头被禁用：`http request headers are unavailable`
- 同时打开的句柄太多：`too many open connections`

## 顶层接口

### isHttpEnabled

- 语法：`internet.isHttpEnabled()`
- 返回：`boolean`
- 用途：返回当前配置是否允许 HTTP 请求。

### request

- 语法：`internet.request(url[, postData[, headers[, method]]])`
- 返回：
  - 成功：`requestHandle`
  - 失败：`nil, reason`
  - 严重失败：抛出异常
- 用途：发起一个 HTTP 或 HTTPS 请求，并返回请求句柄。

参数说明：

- `url:string`：HTTP 或 HTTPS URL。
- `postData:string` 可选：请求体。提供后通常会隐式使用 POST，除非 `method` 明确覆盖。
- `headers:table` 可选：字符串到字符串的请求头表。
- `method:string` 可选：显式指定 HTTP 方法，例如 `GET`、`POST`、`PUT`。

行为说明：

- 只接受 `http` 和 `https`。
- 非法 URL 会抛出 `invalid address`。
- 不支持的协议会抛出 `unsupported protocol`。
- 被访问控制规则阻止的地址会抛出 `address is not allowed`。
- 回调文档字符串中提到会使用 `http_response` 信号，但当前 Scala 实现本身并没有显式发送这个信号。实际使用时应当轮询返回的请求句柄，通过 `finishConnect()`、`response()` 和 `read()` 读取结果。

示例：

```lua
local req, reason = internet.request("https://example.com")
if not req then
  error(reason)
end
```

### isTcpEnabled

- 语法：`internet.isTcpEnabled()`
- 返回：`boolean`
- 用途：返回当前配置是否允许原始 TCP 连接。

### connect

- 语法：`internet.connect(address[, port])`
- 返回：
  - 成功：`socketHandle`
  - 失败：`nil, reason`
  - 严重失败：抛出异常
- 用途：打开一个非阻塞 TCP 套接字句柄。

参数说明：

- `address:string`：主机名、`host:port`，或 URI 风格地址。
- `port:number` 可选：当 `address` 本身未包含端口时，作为后备端口使用。

行为说明：

- 主机地址必须能成功解析，并且最终 IP 必须通过允许列表检查。
- 如果 `address` 和可选第二参数都没有提供有效端口，则会抛出 `address could not be parsed or no valid port given`。

示例：

```lua
local socket = assert(internet.connect("example.com", 80))
```

## TCP 套接字句柄

`internet.connect(...)` 返回的套接字值带有以下回调。

### finishConnect

- 语法：`socket.finishConnect()`
- 返回：`boolean`
- 用途：轮询套接字，直到连接完成或失败。

行为说明：

- 当 DNS 解析或非阻塞连接仍在进行中时，返回 `false`。
- 连接成功建立后返回 `true`。
- 内部连接失败时，该描述符会被关闭，后续操作会表现为连接已丢失或句柄已关闭。

### read

- 语法：`socket.read([n])`
- 返回：
  - 已连接且有数据：`string`
  - 已连接但暂时没有数据：空字符串
  - 流结束：`nil`
- 用途：从套接字读取字节数据。

参数说明：

- `n:number` 可选：请求读取的字节数，最终会被限制到 `Settings.get.maxReadBuffer`。

行为说明：

- 该回调返回原始字节，Lua 中会表现为字节字符串。
- 如果套接字尚未连接完成，则实现会返回空字节数组。
- 如果对端已经关闭流，则返回 `nil`。

### write

- 语法：`socket.write(data)`
- 返回：`number`
- 用途：向套接字写入原始字节。

参数说明：

- `data:string`：要写入的字节字符串。

行为说明：

- 返回本次成功写入的字节数。
- 如果套接字尚未连接完成，则返回 `0`。

### close

- 语法：`socket.close()`
- 返回：无可依赖的有效返回值
- 用途：立即关闭该套接字句柄。

### id

- 语法：`socket.id()`
- 返回：`string`
- 用途：返回该句柄用于就绪通知的 UUID。

### TCP 就绪信号

当选择器发现该套接字有可读数据时，网卡会发送：

```lua
computer.signal("internet_ready", socketId)
```

你可以通过 `socket.id()` 返回的值来匹配具体句柄。

## HTTP 请求句柄

`internet.request(...)` 返回的请求值带有以下回调。

### finishConnect

- 语法：`request.finishConnect()`
- 返回：`boolean`
- 用途：轮询直到 HTTP 响应头已经可用。

行为说明：

- 请求仍在处理中时返回 `false`。
- 响应头就绪后返回 `true`。
- 网络和协议错误会以 IO 异常形式抛出，例如：
  - `unknown host: ...`
  - `address is not allowed`
  - 其他被包装后的传输错误文本

### response

- 语法：`request.response()`
- 返回：
  - 尚未就绪时：`nil`
  - 就绪后：`code, message, headers`
- 用途：返回响应元数据。

返回值说明：

- `code:number`：HTTP 状态码
- `message:string`：HTTP 状态消息
- `headers:table`：转换成 Lua 的 Java 响应头表

### read

- 语法：`request.read([n])`
- 返回：
  - 有正文数据时：`string`
  - 仍在等待时：空字符串
  - 正文结束时：`nil`
- 用途：读取响应体字节流。

参数说明：

- `n:number` 可选：请求读取的字节数，最终会被限制到 `Settings.get.maxReadBuffer`。

行为说明：

- 到达文件结束且内部队列也为空时，会返回 `nil`。
- 如果当前没有已经缓冲好的字节，实现会安排后台继续读取，并先返回当前已有的数据。

### close

- 语法：`request.close()`
- 返回：无可依赖的有效返回值
- 用途：取消并关闭请求句柄。

## 示例

```lua
local internet = require("component").internet

local req = assert(internet.request("https://example.com"))
while not req.finishConnect() do
  os.sleep(0.05)
end

local code, message, headers = req.response()
print(code, message)

local body = {}
while true do
  local chunk = req.read(1024)
  if not chunk then
    break
  end
  if #chunk > 0 then
    body[#body + 1] = chunk
  else
    os.sleep(0.05)
  end
end

req.close()
print(table.concat(body))
```

## 相关内容

- `component`
- `opencomputers-network-card`
