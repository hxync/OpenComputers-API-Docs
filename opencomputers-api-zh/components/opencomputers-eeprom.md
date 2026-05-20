# eeprom

## 概述

`eeprom` 组件用于控制 EEPROM 芯片中的引导代码、标签、校验和、只读状态，以及额外的易失数据区。它既是电脑启动时加载 BIOS 风格代码的地方，也是无人机、微控制器等设备最核心的可编程存储介质之一。

这个组件实际上暴露了两块不同的字节数组：

- 主 EEPROM 代码区；
- 一块独立的附加易失数据区。

这两块区域的容量不同、行为不同，只读限制也不完全一样。

## 可用性

- 仓库：`opencomputers`
- 组件名：`eeprom`
- 常见宿主：EEPROM 物品、电脑、无人机、微控制器、平板、服务器

## 重要说明

- `get` / `set` 操作的是主引导代码区；
- `getData` / `setData` 操作的是附加易失数据区；
- `makeReadonly` 会永久锁定主代码区和标签，之后不能再改；
- 但源码里 `setData` 在只读后依然允许写入附加数据区。

## 用法

```lua
local component = require("component")
local eeprom = component.eeprom

if not eeprom then
  error("当前环境没有 eeprom 组件")
end
```

## 接口

### get

- 语法：`eeprom.get()`
- 返回值：`string`
- 用途：读取当前主 EEPROM 代码区中保存的字节数组。

这里存放的就是启动程序或引导代码本体。

示例：

```lua
local code = eeprom.get()
print("代码字节数:", #code)
```

### set

- 语法：`eeprom.set(data)`
- 返回值：
  - 成功：没有直接返回值
  - 失败：`nil, reason`
- 用途：整体替换主 EEPROM 代码区中的字节数组。

参数说明：

- `data:string` 可选：要写入的原始字节内容。省略时会写入空字节数组。

行为说明：

- 如果 EEPROM 已经锁成只读，会返回 `nil, "storage is readonly"`；
- 能量不足时会返回 `nil, "not enough energy"`；
- 如果数据超过 EEPROM 主代码区容量，会直接抛出 `"not enough space"`；
- 源码中该调用会让电脑暂停 `2` 秒。

### getLabel

- 语法：`eeprom.getLabel()`
- 返回值：`string`
- 用途：读取 EEPROM 标签。

### setLabel

- 语法：`eeprom.setLabel(data)`
- 返回值：
  - 成功：`string`
  - 失败：`nil, reason`
- 用途：修改 EEPROM 标签，并返回最终存入的标签值。

参数说明：

- `data:string` 可选：新标签文本。

行为说明：

- 进入只读状态后会返回 `nil, "storage is readonly"`；
- 标签会先去掉首尾空白，再截断到 24 个字符；
- 如果结果为空，则会自动恢复成 `"EEPROM"`。

### getSize

- 语法：`eeprom.getSize()`
- 返回值：`number`
- 用途：读取主 EEPROM 代码区容量，单位为字节。

### getChecksum

- 语法：`eeprom.getChecksum()`
- 返回值：`string`
- 用途：读取当前主 EEPROM 代码数据的 CRC32 校验和。

`makeReadonly` 就是用这个校验和值来做确认的。

### makeReadonly

- 语法：`eeprom.makeReadonly(checksum)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：把 EEPROM 永久切换为只读状态。

参数说明：

- `checksum:string`：通常就是 `getChecksum()` 返回的当前校验和。

行为说明：

- 一旦成功，这个过程不可逆；
- 如果传入的校验和与当前代码数据不匹配，会返回 `nil, "incorrect checksum"`。

示例：

```lua
local checksum = eeprom.getChecksum()
local ok, reason = eeprom.makeReadonly(checksum)
print(ok, reason)
```

### getDataSize

- 语法：`eeprom.getDataSize()`
- 返回值：`number`
- 用途：读取附加易失数据区的容量。

它和 `getSize()` 返回的主程序容量不是同一块空间。

### getData

- 语法：`eeprom.getData()`
- 返回值：`string`
- 用途：读取附加易失字节数组。

这块区域通常用于保存少量希望跟随 EEPROM 一起移动的额外状态。

### setData

- 语法：`eeprom.setData(data)`
- 返回值：
  - 成功：没有直接返回值
  - 失败：`nil, reason`
- 用途：整体替换附加易失字节数组。

参数说明：

- `data:string` 可选：要写入的原始字节内容。

行为说明：

- 与 `set` 不同，源码里的 `setData` 不受只读标志限制；
- 能量不足时返回 `nil, "not enough energy"`；
- 如果数据超过附加数据区容量，会直接抛出 `"not enough space"`；
- 源码中该调用会让电脑暂停 `1` 秒。

示例：

```lua
eeprom.setData("cfg=1")
print("附加数据字节数:", #eeprom.getData())
```

### 引导代码示例

```lua
local program = [[
return function()
  computer.beep(1000, 0.1)
end
]]

local ok, reason = eeprom.set(program)
if ok == nil then
  io.stderr:write("EEPROM 写入失败: " .. tostring(reason) .. "\n")
end
```

## 相关内容

- `computer`
- `drive`
- `filesystem`
- `component`
