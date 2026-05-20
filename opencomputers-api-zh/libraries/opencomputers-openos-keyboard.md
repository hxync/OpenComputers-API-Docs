# keyboard

## 概述

跟踪已按下的按键，并检测常见修饰键或控制键状态。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\keyboard.lua`

## 用法

```lua
local keyboard = require("keyboard")
```

## 接口

### keyboard.isAltDown()

- 说明: 返回当前是否按下了任意一个 Alt 键。

```lua
keyboard.isAltDown()
```

### keyboard.isControl(char)

- 说明: 返回某个数字字符码是否属于控制字符范围。
- 参数:
  - `char`

```lua
keyboard.isControl(nil)
```

### keyboard.isControlDown()

- 说明: 返回当前是否按下了任意一个 Control 键。

```lua
keyboard.isControlDown()
```

### keyboard.isKeyDown(charOrCode)

- 说明: 返回指定字符或按键码当前是否被标记为已按下。
- 参数:
  - `charOrCode`

```lua
keyboard.isKeyDown(nil)
```

### keyboard.isShiftDown()

- 说明: 返回当前是否按下了任意一个 Shift 键。

```lua
keyboard.isShiftDown()
```

## 备注

- 这个模块还会暴露一个 `keys` 表；在引导阶段它只包含最小子集，完整键位映射会在真正访问时延迟加载。
