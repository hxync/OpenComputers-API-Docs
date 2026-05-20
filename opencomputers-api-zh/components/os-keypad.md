# os_keypad

## 概述

`os_keypad` 组件用于控制 OpenSecurity 的密码键盘锁。它可以修改按键事件名、开关按键蜂鸣声、自定义显示屏文本，以及重写 12 个按键的标签和颜色。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_keypad`
- 常见宿主：放置在世界中的 OpenSecurity 密码键盘

## 用法

```lua
local component = require("component")
local keypad = component.os_keypad

if not keypad then
  error("未安装 os_keypad 组件")
end
```

## 按键信号

当玩家按下按键时，键盘会发送：

```lua
computer.signal(eventName, buttonIndex, label)
```

默认事件名：

- `keypad`

载荷字段说明：

- `buttonIndex:number`：`1` 到 `12`
- `label:string`：该按键当前显示的文本标签

默认按键标签布局：

- `1 2 3`
- `4 5 6`
- `7 8 9`
- `* 0 #`

## 文本与颜色限制

- 显示屏文本最长保留 `8` 个字符。
- 按键标签最长保留 `3` 个字符。
- 颜色值会经过 `& 7` 掩码处理，因此只有最低三位会生效。
- 也就是说，实际有效颜色范围是 `0` 到 `7`。

## 接口

### setEventName

- 语法：`keypad.setEventName(name)`
- 返回：`boolean`
- 用途：修改按键按下时发送的事件名。

参数说明：

- `name:string`：新的事件名。

示例：

```lua
assert(keypad.setEventName("doorPad"))
```

### setShouldBeep

- 语法：`keypad.setShouldBeep(enabled)`
- 返回：`boolean`
- 用途：开启或关闭按键按下时的蜂鸣音。

参数说明：

- `enabled:boolean`：`true` 表示启用蜂鸣，`false` 表示静音。

行为说明：

- 默认值为 `true`。
- 这个设置会保存到 NBT 中。

示例：

```lua
assert(keypad.setShouldBeep(false))
```

### setDisplay

- 语法：`keypad.setDisplay(text[, color])`
- 返回：`boolean`
- 用途：更新键盘显示屏文本以及可选的显示颜色。

参数说明：

- `text:string`：显示到屏幕上的文本，超过 `8` 个字符会被截断。
- `color:number` 可选：显示颜色值，最终会被限制到 `0` 到 `7`。

示例：

```lua
assert(keypad.setDisplay("OPEN", 2))
```

### setKey

- 语法：
  - `keypad.setKey(index, text[, color])`
  - `keypad.setKey(labelTable[, colorTable])`
- 返回：`boolean`
- 用途：修改单个按键标签，或一次性重写整套键盘布局。

单键模式参数说明：

- `index:number`：按键编号，范围 `1` 到 `12`
- `text:string`：新标签，超过 `3` 个字符会被截断
- `color:number` 可选：按键颜色，最终会被限制到 `0` 到 `7`

表模式参数说明：

- `labelTable:table`：以 `1` 到 `12` 为键的标签表
- `colorTable:table` 可选：以 `1` 到 `12` 为键的颜色表

行为说明：

- 编号超出范围时会抛出 `Index <n> is out of range`。
- 第一个参数类型不正确时会抛出 `First argument must be index or table`。

示例：

```lua
assert(keypad.setKey(10, "CLR", 4))
assert(keypad.setKey({
  [1] = "A",
  [2] = "B",
  [3] = "C"
}, {
  [1] = 1,
  [2] = 2,
  [3] = 4
}))
```

## 示例

```lua
local event = require("event")

keypad.setEventName("doorPad")
keypad.setDisplay("LOCKED", 4)
keypad.setShouldBeep(true)

while true do
  local _, _, _, eventName, index, label = event.pull("computer.signal")
  if eventName == "doorPad" then
    print(index, label)
  end
end
```

## 相关内容

- `component`
- `os_door`
