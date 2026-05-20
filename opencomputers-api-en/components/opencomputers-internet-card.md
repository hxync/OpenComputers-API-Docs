# internet

## Summary

This page documents the OpenComputers internet card component. The generated filename is `opencomputers-internet-card.md`, but the real component name exposed to Lua is `internet`.

The component can:

- open TCP sockets
- start HTTP requests
- return per-connection handle objects for polling and I/O

## Availability

- Repository: `opencomputers`
- Actual component name: `internet`
- Typical host: OpenComputers internet card

## Usage

```lua
local component = require("component")
local internet = component.internet

if not internet then
  error("internet component not installed")
end
```

## Ownership Rules

- The card binds itself to the first neighboring computer context that connects to it.
- All later callback calls must come from that owning computer.
- Otherwise the implementation throws `can only be used by the owning computer`.
- When the owning computer stops, starts, or disconnects, all open internet handles are closed.

## Configuration Gates

The card is also limited by OpenComputers config:

- no internet at all: `internet access is unavailable`
- HTTP disabled: `http requests are unavailable`
- TCP disabled: `tcp connections are unavailable`
- custom HTTP headers disabled: `http request headers are unavailable`
- too many simultaneous handles: `too many open connections`

## Top-Level API

### isHttpEnabled

- Syntax: `internet.isHttpEnabled()`
- Returns: `boolean`
- Purpose: Returns whether HTTP requests are enabled by config.

### request

- Syntax: `internet.request(url[, postData[, headers[, method]]])`
- Returns:
  - Success: `requestHandle`
  - Failure: `nil, reason`
  - Severe failure: exception
- Purpose: Starts an HTTP or HTTPS request and returns a request handle.

Parameters:

- `url:string`: HTTP or HTTPS URL.
- `postData:string` optional: request body. Supplying this usually implies POST unless `method` overrides it.
- `headers:table` optional: string-to-string header table.
- `method:string` optional: explicit HTTP method such as `GET`, `POST`, or `PUT`.

Behavior:

- Only `http` and `https` are accepted.
- Invalid URLs raise `invalid address`.
- Unsupported schemes raise `unsupported protocol`.
- Blocked destinations raise `address is not allowed`.
- The callback docstring mentions `http_response` signals, but this implementation does not explicitly emit such a signal from the Scala source. In practice, the returned request handle is meant to be polled with `finishConnect()`, `response()`, and `read()`.

Example:

```lua
local req, reason = internet.request("https://example.com")
if not req then
  error(reason)
end
```

### isTcpEnabled

- Syntax: `internet.isTcpEnabled()`
- Returns: `boolean`
- Purpose: Returns whether raw TCP sockets are enabled by config.

### connect

- Syntax: `internet.connect(address[, port])`
- Returns:
  - Success: `socketHandle`
  - Failure: `nil, reason`
  - Severe failure: exception
- Purpose: Opens a non-blocking TCP socket handle.

Parameters:

- `address:string`: host, host:port, or URI-style address.
- `port:number` optional: fallback port if it is not embedded in `address`.

Behavior:

- The host must parse successfully and must resolve to an allowed address.
- If neither the address string nor the optional second argument provide a valid port, the implementation throws `address could not be parsed or no valid port given`.

Example:

```lua
local socket = assert(internet.connect("example.com", 80))
```

## TCP Socket Handle

`internet.connect(...)` returns a socket value with the following callbacks.

### finishConnect

- Syntax: `socket.finishConnect()`
- Returns: `boolean`
- Purpose: Polls the socket until the connection either completes or fails.

Behavior:

- Returns `false` while DNS resolution or the non-blocking connect is still in progress.
- Returns `true` once the socket is connected.
- On internal connection failure the descriptor is closed and later operations behave as lost or closed.

### read

- Syntax: `socket.read([n])`
- Returns:
  - Connected and data available: `string`
  - Connected but no data yet: empty string
  - End of stream: `nil`
- Purpose: Reads bytes from the socket.

Parameters:

- `n:number` optional: requested byte count, clamped to `Settings.get.maxReadBuffer`.

Behavior:

- The callback returns raw bytes, which Lua receives as a byte string.
- If the socket is not connected yet, the implementation returns an empty byte array.
- If the peer closed the stream, the callback returns `nil`.

### write

- Syntax: `socket.write(data)`
- Returns: `number`
- Purpose: Writes raw bytes to the socket.

Parameters:

- `data:string`: byte string to write.

Behavior:

- Returns the number of bytes written.
- Returns `0` if the socket is not yet connected.

### close

- Syntax: `socket.close()`
- Returns: no meaningful value
- Purpose: Closes the socket handle immediately.

### id

- Syntax: `socket.id()`
- Returns: `string`
- Purpose: Returns the handle UUID used by readiness notifications.

### TCP readiness signal

When the selector notices readable socket data, the card sends:

```lua
computer.signal("internet_ready", socketId)
```

Use `socket.id()` to match the reported handle.

## HTTP Request Handle

`internet.request(...)` returns a request value with the following callbacks.

### finishConnect

- Syntax: `request.finishConnect()`
- Returns: `boolean`
- Purpose: Polls until the HTTP response headers are available.

Behavior:

- Returns `false` while the request is still in progress.
- Returns `true` once response headers are ready.
- Network and protocol failures are raised as IO exceptions such as:
  - `unknown host: ...`
  - `address is not allowed`
  - other wrapped transport errors

### response

- Syntax: `request.response()`
- Returns:
  - Before ready: `nil`
  - After ready: `code, message, headers`
- Purpose: Returns response metadata.

Returned values:

- `code:number`: HTTP status code
- `message:string`: HTTP status message
- `headers:table`: Java header map converted to Lua

### read

- Syntax: `request.read([n])`
- Returns:
  - Body bytes available: `string`
  - Still pending: empty string
  - End of body: `nil`
- Purpose: Reads response body bytes.

Parameters:

- `n:number` optional: requested byte count, clamped to `Settings.get.maxReadBuffer`.

Behavior:

- Once end-of-file is reached and the queue is empty, the callback returns `nil`.
- If no bytes are currently buffered, the implementation schedules another background read and returns what is currently available.

### close

- Syntax: `request.close()`
- Returns: no meaningful value
- Purpose: Cancels and closes the request handle.

## Example

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

## Related

- `component`
- `opencomputers-network-card`
