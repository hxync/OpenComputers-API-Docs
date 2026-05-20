# os_cardwriter

## 概述

`os_cardwriter` 是 OpenSecurity 提供的卡片与 EEPROM 写入站组件。它既可以把空白 RFID 卡或磁条卡写成成品卡，也可以把输入槽中的 EEPROM 烧录成新的输出副本。

这个方块还会为输入槽插卡和拔卡发送事件，并附带一个名为 `cardwriter` 的文件系统环境。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_cardwriter`
- 常见宿主：放置在世界中的 OpenSecurity 卡片写入器

## 用法

```lua
local component = require("component")
local writer = component.os_cardwriter

if not writer then
  error("未安装 os_cardwriter 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 物品栏布局

- 输入槽：内部槽位 `0`
- 回调使用的输出槽：
  - 内部槽位 `3` 到 `12`

只有当输入槽存在可写入的卡片或 EEPROM，且至少有一个输出槽为空时，这些回调才会成功工作。

## 事件

当槽位 `0` 状态变化时，方块会发送以下 `computer.signal` 事件：

- `cardInsert`, `"cardInsert"`
- `cardRemove`, `"cardRemove"`

## 接口

### write

- 语法：`writer.write(data[, displayName[, locked[, color]]])`
- 返回值：
  - 成功：`true, uuid`
  - 失败：`false, reason`
- 用途：把数据写入空白 RFID 卡或磁条卡，并把写好的成品放进输出槽。

参数说明：

- `data:string`：写入卡中的数据内容。
- `displayName:string` 可选：生成卡片在物品栏中的显示名称。
- `locked:boolean` 可选：是否把成品卡标记为锁定，阻止再次改写。
- `color:number` 可选：OpenSecurity 使用的颜色编号，范围 `0` 到 `15`。

行为说明：

- 每次真正进入电力检查的写卡操作会消耗 `5` 点缓冲能量。
- 输入卡要求：
  - 空白 RFID 卡，或
  - 空白磁条卡
- 数据长度限制：
  - RFID：`64` 个字符
  - 磁条卡：`128` 个字符
- 超出限制的内容会被静默截断。
- 如果成品卡的标签数据里还没有 UUID，系统会自动写入一个新的 UUID。

颜色映射：

- `0` 白色
- `1` 橙色
- `2` 品红
- `3` 浅蓝
- `4` 黄色
- `5` 黄绿色
- `6` 粉色
- `7` 灰色
- `8` 浅灰 / 银色
- `9` 青色
- `10` 紫色
- `11` 蓝色
- `12` 棕色
- `13` 绿色
- `14` 红色
- `15` 黑色

常见失败：

- `false, "Not enough power in OC Network."`
- `false, "No card in slot"`
- `false, "No Empty Slots"`

示例：

```lua
local ok, uuidOrReason = writer.write("sector-a", "Sector A Access", true, 14)
if not ok then
  error(uuidOrReason)
end
print("卡片 UUID:", uuidOrReason)
```

### flash

- 语法：`writer.flash(code, title, locked)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：使用输入槽中的 EEPROM 在空输出槽中烧录一个新的 EEPROM 副本。

参数说明：

- `code:string`：要写入 EEPROM 的 UTF-8 字节内容。
- `title:string`：EEPROM 标签名称。
- `locked:boolean`：是否把生成的 EEPROM 标记为只读。

行为说明：

- 输入槽 `0` 中的物品必须是 EEPROM。
- 系统会在第一个空输出槽生成一个新的 EEPROM 物品。
- 输入槽中的原始 EEPROM 会消耗 1 个。
- 数据长度：
  - 普通配置下限制为 `Settings.get().eepromSize()`
  - 启用大 EEPROM 配置时限制为 `8096` 字节
- 标签长度：
  - 普通配置下限制为 `Settings.get().eepromDataSize()`
  - 启用大 EEPROM 配置时限制为 `512` 个字符
- 超出限制的内容会被静默截断。

常见失败：

- `false, "No EEPROM in slot"`
- `false, "Item is not EEPROM"`
- `false, "No Empty Slots"`
- `false, "Data is Null"`

示例：

```lua
local ok, reason = writer.flash("print('hello')", "Greeter", true)
if not ok then
  error(reason)
end
```

## 说明

- 这个方块还会额外挂载一个名为 `cardwriter` 的文件系统环境，内容来自随模组附带的 Lua 资源。
- 输入槽只接受空白且未锁定的输入介质。

## 相关内容

- `component`
- `os_biometric`
