# opencomputers-upgrade-sign-in-rotatable

## 概述

本页记录机器人等可旋转宿主在安装告示牌升级后暴露出的 `sign` 组件接口。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`sign`
- 常见宿主：安装了告示牌升级的机器人，以及其他可旋转宿主

## 作用

- 读取宿主正前方告示牌上的文本。
- 向宿主正前方的告示牌写入新文本。

它和适配器版本的告示牌接口使用相同的文本格式规则与权限检查逻辑。

## 文本规则

- 告示牌文本会表示为一个带换行的单字符串。
- 写入时最多只保留四行。
- 每行最多保留 `15` 个字符。

## API

### getValue

- 语法：`sign.getValue()`
- 返回值：
  - 成功：`string`
  - 失败：`nil, "no sign"`
- 用途：读取宿主正前方告示牌上的文本。

### setValue

- 语法：`sign.setValue(value)`
- 返回值：
  - 成功：`string`
  - 失败：`nil, reason`
- 用途：向宿主正前方的告示牌写入文本，并返回实际写入后的结果。

可能失败原因：

- `no sign`
- `not allowed`

## 示例

```lua
local component = require("component")
local sign = component.sign

print("Current sign text:", sign.getValue())
print("Updated sign text:", sign.setValue("Dock A\nInput"))
```

## 相关内容

- `opencomputers-upgrade-sign-in-adapter`
- `robot`
