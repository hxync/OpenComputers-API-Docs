# adapter_api

## 概述

供 devfs 硬件启动器使用的内部辅助 API，用来构建可读写的适配器文件。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\devfs\01_hw.lua`

## 用法

```lua
-- internal devfs starter helpers passed into adapter modules
```

```lua
local starter = dofile("/lib/core/devfs/01_hw.lua")
```

## 接口

### adapter_api.create_toggle(read, write, switch)

- 说明: 构建一个小型 `{read=..., write=...}` 适配器，把布尔状态暴露为 `true`/`false` 文本，并接受 `0/1/true/false` 写入。
- 参数:
  - `read`: 可选，用来读取当前布尔状态的回调。
  - `write`: 用于写入启用或开启状态的回调。
  - `switch`: 可选；如果存在显式关闭动作，会优先调用它，而不是执行 `write(false)`。

```lua
local toggle = adapter_api.create_toggle(proxy.isOn, proxy.turnOn, proxy.turnOff)
```

### adapter_api.createWriter(callback, ...)

- 说明: 包装一个组件回调，使其能够先把空白分隔的文本行解析为带类型的参数，再进行调用。
- 参数:
  - `callback`: 将接收解析后参数的函数。
  - `...`: 参数声明包：先写必需参数个数，再写每个参数的类型名，例如 `number` 或 `boolean`。

```lua
local writeDepth = adapter_api.createWriter(proxy.setDepth, 1, 'number')
```

### adapter_api.make_link(list, addr, prefix, bOmitZero)

- 说明: 向某个列表表中插入唯一的符号链接式条目，并为给定前缀选择第一个可用的数字后缀。
- 参数:
  - `list`: 接收生成链接条目的表。
  - `addr`: 写入到生成条目中的链接目标路径。
  - `prefix`: 可选，生成名称时使用的文本前缀。
  - `bOmitZero`: 为真时，第一个生成名称直接使用裸前缀，而不是附加后缀 `0`。

```lua
adapter_api.make_link(typeDir.list, '../../by-address/' .. addr)
```

### adapter_api.toArgsPack(input, pack)

- 说明: 把一行输入拆成 token，并按类型声明强制转换为一个打包参数列表。
- 参数:
  - `input`: 要解析的原始文本输入行。
  - `pack`: 类型声明表；其第一个元素表示最少需要的参数个数。

```lua
local args, why = adapter_api.toArgsPack('80 25', {2, 'number', 'number'})
```

## 备注

- 本页记录的是导出的 `adapter_api` 辅助表；而启动文件本身实际返回的是 `/dev/components` 顶层树描述。
