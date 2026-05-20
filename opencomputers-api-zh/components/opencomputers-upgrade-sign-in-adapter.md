# opencomputers-upgrade-sign-in-adapter

## 概述

本页记录适配器在与周围告示牌配合使用时暴露出的 `sign` 组件接口。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`sign`
- 常见宿主：适配器方块

## 作用

- 读取适配器指定侧面告示牌上的文本。
- 向适配器指定侧面的告示牌写入文本。
- 如果宿主自身就是告示牌方块实体，也可以直接针对宿主本体工作。

## 文本规则

- 读取时会把告示牌文本按换行符拼接成一个字符串返回。
- 写入时最多只保留四行文本。
- 每一行最多保留 `15` 个字符，超出的部分会被截断。
- 不足四行的部分会自动补成空字符串。

## API

### getValue

- 语法：`sign.getValue(side)`
- 返回值：
  - 成功：`string`
  - 失败：`nil, "no sign"`
- 用途：读取指定侧面告示牌上的文本。

### setValue

- 语法：`sign.setValue(side, value)`
- 返回值：
  - 成功：`string`
  - 失败：`nil, reason`
- 用途：向指定侧面的告示牌写入文本，并返回最终实际写入的文本。

可能失败原因：

- `no sign`
- `not allowed`

其中 `not allowed` 表示伪玩家权限检查或告示牌变更事件阻止了这次修改。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local sign = component.sign

local text, reason = sign.getValue(sides.north)
print("Current text:", text, reason)

local updated, err = sign.setValue(sides.north, "Line 1\nLine 2")
print("Updated text:", updated, err)
```

## 相关内容

- `opencomputers-upgrade-sign-in-rotatable`
- `adapter`
