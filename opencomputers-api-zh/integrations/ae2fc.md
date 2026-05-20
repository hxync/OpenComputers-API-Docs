# ae2fc

## 概述

本页说明 OpenComputers 暴露给 Lua 的 AE2 Fluid Craft 集成接口。它在 Applied Energistics 风格 API 的基础上，增加了流体导出总线、流体导入总线、流体存储总线和流体接口。

## 可用性

- 依赖: `ae2fc`
- 标签: `integration-required`

## 组件名

- `fluid_exportbus`
- `fluid_importbus`
- `fluid_storagebus`
- `fluid_interface`

## 主要用途

- 读取和修改 AE2 Fluid Craft 各类总线上的流体过滤槽。
- 用流体描述表配置流体接口。
- 在处理流体而不是物品时，继续沿用熟悉的 AE2 总线操作方式。
- 通过流体注册名、流体量和可选栈大小构建自动化逻辑。

## 流体描述表

本页大多数写入接口都接受流体描述表。这些表会通过 OpenComputers 的流体转换器进行解析。

常用字段如下:

- `name: string`
  流体注册名，例如 `water`、`lava` 或其他模组流体 id。
- `id: number`
  可选的数字流体 id，可以代替 `name` 使用。
- `amount: number`
  流体数量。解析时如果省略，默认是 `0`。
- `size: number`
  可选 AE 栈大小。省略时默认是 `1`。

说明:

- 必须至少提供 `name` 或 `id` 其中之一，否则会抛出 `fluid id or name expected`。
- 如果提供的流体 id 或流体名不存在，会抛出 `fluid not found`。
- 不传描述表时，会清空目标配置槽。
- 返回表由 OC 的流体转换器和 AE 栈包装器共同生成，常见字段如下：
  - `name: string`，流体注册名。
  - `label: string`，流体本地化名称。
  - `amount: number`，流体量。
  - `hasTag: boolean`，底层 `FluidStack` 是否带 NBT。
  - `size: number`，AE 栈大小，未显式设置时默认为 `1`。
  - `isCraftable: boolean`，该 AE 栈是否被标记为可合成。
- 只有在 `insertIdsInConverters` 配置开启时，返回表里才会额外包含 `id: number`。

## 组件

### fluid_exportbus

`fluid_exportbus` 用于 AE2 Fluid Craft 的流体导出总线部件。

#### `getExportConfiguration(side[, slot])`

- 语法: `getExportConfiguration(side: number[, slot: number]): table`
- 用途: 返回某个导出槽里配置的流体描述。
- 参数:
  - `side: number`
    安装该流体导出总线的 multipart 方向。
  - `slot: number`
    可选配置槽编号。
- 返回值:
  - 目标槽位中的流体描述表。
- 说明:
  - 如果省略 `slot`，默认读取 `0` 号槽位。
  - 槽位编号非法时会抛出 `invalid slot`。
  - 返回表结构遵循上面的流体描述表格式。

#### `setExportConfiguration(side[, slot][, detail])`

- 语法: `setExportConfiguration(side: number[, slot: number][, detail: table]): boolean`
- 用途: 设置或清空一个导出过滤槽。
- 参数:
  - `side: number`
    安装该流体导出总线的 multipart 方向。
  - `slot: number`
    可选配置槽编号。
  - `detail: table`
    可选流体描述表。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 省略 `detail` 时会清空该槽位。
  - 槽位编号非法时会抛出 `invalid slot`。
  - 虽然内部辅助类名称仍沿用 item bus 命名，但这里处理的实际是流体描述。

#### `getExportSlotSize(side)`

- 语法: `getExportSlotSize(side: number): number`
- 用途: 返回该总线可用的配置槽数量。
- 参数:
  - `side: number`
    安装该流体导出总线的 multipart 方向。
- 返回值:
  - 有效配置槽数量。

#### `getExportOreFilter(side)`

- 语法: `getExportOreFilter(side: number): string`
- 用途: 返回总线当前使用的过滤字符串。
- 参数:
  - `side: number`
    安装该流体导出总线的 multipart 方向。
- 返回值:
  - 当前过滤字符串。
- 说明:
  - 即使是流体总线，接口命名仍沿用了 AE 总线旧有的 ore filter 叫法。

#### `setExportOreFilter(side, filter)`

- 语法: `setExportOreFilter(side: number, filter: string): boolean`
- 用途: 修改总线过滤字符串。
- 参数:
  - `side: number`
    安装该流体导出总线的 multipart 方向。
  - `filter: string`
    新过滤字符串。
- 返回值:
  - `true` 表示成功。

### fluid_importbus

`fluid_importbus` 用于 AE2 Fluid Craft 的流体导入总线部件。

#### `getImportConfiguration(side[, slot])`

- 语法: `getImportConfiguration(side: number[, slot: number]): table`
- 用途: 返回某个导入槽里配置的流体描述。
- 参数:
  - `side: number`
    安装该流体导入总线的 multipart 方向。
  - `slot: number`
    可选配置槽编号。
- 返回值:
  - 流体描述表。
- 说明:
  - 如果省略 `slot`，默认读取 `0` 号槽位。
  - 槽位编号非法时会抛出 `invalid slot`。

#### `setImportConfiguration(side[, slot][, detail])`

- 语法: `setImportConfiguration(side: number[, slot: number][, detail: table]): boolean`
- 用途: 设置或清空一个导入过滤槽。
- 参数:
  - `side: number`
    安装该流体导入总线的 multipart 方向。
  - `slot: number`
    可选配置槽编号。
  - `detail: table`
    可选流体描述表。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 省略 `detail` 时会清空目标槽位。
  - 槽位编号非法时会抛出 `invalid slot`。

#### `getImportSlotSize(side)`

- 语法: `getImportSlotSize(side: number): number`
- 用途: 返回导入总线有效配置槽数量。
- 参数:
  - `side: number`
    安装该流体导入总线的 multipart 方向。
- 返回值:
  - 槽位数量。

#### `getImportOreFilter(side)`

- 语法: `getImportOreFilter(side: number): string`
- 用途: 返回当前导入总线过滤字符串。
- 参数:
  - `side: number`
    安装该流体导入总线的 multipart 方向。
- 返回值:
  - 当前过滤字符串。

#### `setImportOreFilter(side, filter)`

- 语法: `setImportOreFilter(side: number, filter: string): boolean`
- 用途: 修改导入总线过滤字符串。
- 参数:
  - `side: number`
    安装该流体导入总线的 multipart 方向。
  - `filter: string`
    新过滤字符串。
- 返回值:
  - `true` 表示成功。

### fluid_storagebus

`fluid_storagebus` 用于 AE2 Fluid Craft 的流体存储总线部件。

#### `getStorageConfiguration(side[, slot])`

- 语法: `getStorageConfiguration(side: number[, slot: number]): table`
- 用途: 返回某个存储槽里配置的流体描述。
- 参数:
  - `side: number`
    安装该流体存储总线的 multipart 方向。
  - `slot: number`
    可选配置槽编号。
- 返回值:
  - 流体描述表。
- 说明:
  - 如果省略 `slot`，默认读取 `0` 号槽位。
  - 槽位编号非法时会抛出 `invalid slot`。

#### `setStorageConfiguration(side[, slot][, detail])`

- 语法: `setStorageConfiguration(side: number[, slot: number][, detail: table]): boolean`
- 用途: 设置或清空一个存储过滤槽。
- 参数:
  - `side: number`
    安装该流体存储总线的 multipart 方向。
  - `slot: number`
    可选配置槽编号。
  - `detail: table`
    可选流体描述表。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 省略 `detail` 时会清空该槽位。
  - 槽位编号非法时会抛出 `invalid slot`。

#### `getStorageOreFilter(side)`

- 语法: `getStorageOreFilter(side: number): string`
- 用途: 返回当前存储总线过滤字符串。
- 参数:
  - `side: number`
    安装该流体存储总线的 multipart 方向。
- 返回值:
  - 当前过滤字符串。

#### `setStorageOreFilter(side, filter)`

- 语法: `setStorageOreFilter(side: number, filter: string): boolean`
- 用途: 修改存储总线过滤字符串。
- 参数:
  - `side: number`
    安装该流体存储总线的 multipart 方向。
  - `filter: string`
    新过滤字符串。
- 返回值:
  - `true` 表示成功。

#### `getStorageSlotSize(side)`

- 语法: `getStorageSlotSize(side: number): number`
- 用途: 返回有效存储配置槽数量。
- 参数:
  - `side: number`
    安装该流体存储总线的 multipart 方向。
- 返回值:
  - 槽位数量。

### fluid_interface

`fluid_interface` 同时存在方块形态和 multipart 形态。它们的方法名相同，但 multipart 版本需要显式传入 `side` 参数。

#### 方块版 `getFluidInterfaceConfiguration([slot])`

- 语法: `getFluidInterfaceConfiguration([slot: number]): table`
- 用途: 返回方块流体接口某个槽位配置的流体描述。
- 参数:
  - `slot: number`
    可选槽位编号，默认是 `0`。
- 返回值:
  - 该槽位的流体描述表。
- 说明:
  - 如果槽位超出接口配置库存范围，底层物品栏会抛出索引错误。

#### 方块版 `setFluidInterfaceConfiguration([slot][, detail])`

- 语法: `setFluidInterfaceConfiguration([slot: number][, detail: table]): boolean`
- 用途: 设置或清空方块流体接口中的某个槽位。
- 参数:
  - `slot: number`
    可选槽位编号，默认是 `0`。
  - `detail: table`
    可选流体描述表。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 省略 `detail` 会清空该槽位。
  - 传入不存在的流体名或 id 时会直接报错，而不是静默失败。

#### multipart 版 `getFluidInterfaceConfiguration(side[, slot])`

- 语法: `getFluidInterfaceConfiguration(side: number[, slot: number]): table`
- 用途: 返回 multipart 流体接口某个槽位配置的流体描述。
- 参数:
  - `side: number`
    安装该接口的 multipart 方向。
  - `slot: number`
    可选槽位编号，默认是 `0`。
- 返回值:
  - 流体描述表。
- 说明:
  - 如果槽位超出接口配置库存范围，底层物品栏会抛出索引错误。

#### multipart 版 `setFluidInterfaceConfiguration(side[, slot][, detail])`

- 语法: `setFluidInterfaceConfiguration(side: number[, slot: number][, detail: table]): boolean`
- 用途: 设置或清空 multipart 流体接口中的某个槽位。
- 参数:
  - `side: number`
    安装该接口的 multipart 方向。
  - `slot: number`
    可选槽位编号，默认是 `0`。
  - `detail: table`
    可选流体描述表。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 省略 `detail` 会清空目标槽位。
  - `side` 必须是确实安装了 `fluid_interface` 的方向，否则会抛出 `no matching part`。

## 示例

```lua
local component = require("component")

if component.isAvailable("fluid_exportbus") then
  local bus = component.fluid_exportbus
  bus.setExportConfiguration(0, 0, {name = "water", amount = 1000, size = 1})
  print("Export slots:", bus.getExportSlotSize(0))
  print("Export filter:", bus.getExportOreFilter(0))
end

if component.isAvailable("fluid_interface") then
  local iface = component.fluid_interface
  iface.setFluidInterfaceConfiguration(0, {name = "lava", amount = 1000})
  local stack = iface.getFluidInterfaceConfiguration(0)
  print(stack and stack.name, stack and stack.amount)
end
```

## 相关内容

- `appeng`
- `thaumicenergistics`
