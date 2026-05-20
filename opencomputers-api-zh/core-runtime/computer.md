# computer

## 概述

`computer` 运行时库向 Lua 程序提供机器标识、内存、能量、信号、用户管理和处理器架构相关接口。它是 OpenComputers 最核心的运行时 API 之一，通常通过 `require("computer")` 加载。

## 可用性

- 仓库：`opencomputers`
- 类型：核心运行时 API

## 用法

```lua
local computer = require("computer")

print("Address:", computer.address())
print("Uptime:", computer.uptime())
print("Free memory:", computer.freeMemory())
```

## 典型用途

- 读取机器运行时长和现实时间。
- 在创建大表或大缓冲区前检查剩余内存。
- 向本地事件队列压入自定义信号。
- 查询和管理授权用户。
- 读取当前网络能量缓存。
- 查看或切换 CPU 支持的架构。

## API

### realTime()

- 语法：`computer.realTime()`
- 返回值：`number`
- 用途：返回现实世界的墙上时钟时间戳，单位为秒。
- 说明：
  - 该值来自宿主 JVM 的系统时间。
  - 适合做超时计算、日志时间戳和与现实时间相关的逻辑。

### uptime()

- 语法：`computer.uptime()`
- 返回值：`number`
- 用途：返回当前计算机自上次启动以来已经运行了多少秒。

### address()

- 语法：`computer.address()`
- 返回值：`string` 或 `nil`
- 用途：返回当前计算机在 OpenComputers 组件网络中的地址。
- 说明：
  - 在极早期启动阶段或节点尚未就绪时可能返回 `nil`。

### freeMemory()

- 语法：`computer.freeMemory()`
- 返回值：`number`
- 用途：返回当前 Lua 可见的剩余可用内存。
- 说明：
  - 这是 Lua 脚本可用的内存额度，不是 JVM 总内存。
  - 返回值已经过当前 Lua 架构内部的内存缩放处理。

### totalMemory()

- 语法：`computer.totalMemory()`
- 返回值：`number`
- 用途：返回当前机器分配给 Lua 的总可见内存额度。

### pushSignal(name, ...)

- 语法：`computer.pushSignal(name, ...)`
- 参数：
  - `name:string`：要压入队列的信号名。
  - `...:any`：可选的信号参数。
- 返回值：`boolean`
- 用途：向本地机器的信号队列压入一个事件。
- 常见用途：
  - 注入自定义应用事件。
  - 唤醒等待 `event.pull()` 的逻辑。
  - 把外部状态变化转发到统一的事件系统。

### tmpAddress()

- 语法：`computer.tmpAddress()`
- 返回值：`string` 或 `nil`
- 用途：返回当前机器临时文件系统的组件地址。
- 说明：
  - 如果机器没有临时文件系统，则返回 `nil`。

### users()

- 语法：`computer.users()`
- 返回值：多个 `string`
- 用途：返回当前机器的授权用户名单。
- 说明：
  - 在 Lua 中通常会用 `{computer.users()}` 把它们收集成数组。

### addUser(user)

- 语法：`computer.addUser(user)`
- 参数：
  - `user:string`：要加入授权列表的玩家名。
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：把玩家加入当前机器的授权用户列表。

### removeUser(user)

- 语法：`computer.removeUser(user)`
- 参数：
  - `user:string`：要移除的玩家名。
- 返回值：`boolean`
- 用途：从授权列表中移除指定玩家。
- 说明：
  - 如果该玩家本来就不在列表中，会返回 `false`。

### energy()

- 语法：`computer.energy()`
- 返回值：`number`
- 用途：返回当前计算机网络可用的全局能量缓存。
- 说明：
  - 如果配置中关闭了能耗限制，返回值可能是正无穷。

### maxEnergy()

- 语法：`computer.maxEnergy()`
- 返回值：`number`
- 用途：返回当前计算机网络能量缓存的最大容量。

### getArchitectures()

- 语法：`computer.getArchitectures()`
- 返回值：`table`
- 用途：返回当前处理器支持的架构名称列表。
- 说明：
  - 可变处理器通常支持多个架构。
  - 固定处理器一般只返回一个架构名。

### getArchitecture()

- 语法：`computer.getArchitecture()`
- 返回值：`string` 或无返回
- 用途：返回当前已经激活的处理器架构名称。

### setArchitecture(name)

- 语法：`computer.setArchitecture(name)`
- 参数：
  - `name:string`：要切换到的架构名。
- 返回值：
  - 成功切换：`true`
  - 目标架构已经处于激活状态：`false`
  - 不支持该架构：`nil, "unknown architecture"`
- 用途：在支持的前提下切换处理器架构。
- 说明：
  - 只有可变处理器才能切换架构。
  - 依赖特定架构行为的程序应先检查返回值。

## 使用示例

```lua
local computer = require("computer")

print("Machine:", computer.address())
print("Running for:", computer.uptime(), "seconds")
print("Memory:", computer.freeMemory(), "/", computer.totalMemory())
print("Energy:", computer.energy(), "/", computer.maxEnergy())

computer.pushSignal("demo_event", "hello", 123)

for _, user in ipairs({computer.users()}) do
  print("Authorized:", user)
end
```

## 相关内容

- `component`
- `event`
- `os`
- `unicode`
