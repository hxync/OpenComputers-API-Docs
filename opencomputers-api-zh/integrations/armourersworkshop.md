# armourersworkshop

## 概述

本页说明 OpenComputers 与 Computronics 为 Armourer's Workshop 提供的人偶姿态接口。它提供一个 `mannequin` 组件，允许 Lua 程序读取并修改人偶各部位的旋转角度。

## 可用性

- 依赖: `armourersworkshop`
- 标签: `integration-required`

## 组件名

- `mannequin`

## 主要用途

- 让展示人偶按照定时器或事件做出动作。
- 在脚本场景结束后把人偶恢复到固定姿态。
- 让装饰姿态跟随红石或其他自动化逻辑变化。

## 组件

### mannequin

`mannequin` 组件对应 Armourer's Workshop 的 mannequin 方块。

#### 有效部位名

所有姿态接口都使用以下小写部位名之一：

- `head`
- `chest`
- `left_arm`
- `right_arm`
- `left_leg`
- `right_leg`

#### getRotation(part)

- 语法: `getRotation(part: string): number, number, number`
- 用途: 返回指定部位当前的 X、Y、Z 三轴旋转角度。
- 参数:
  - `part`: 目标部位名。
- 返回值:
  - 三个角度值，单位都是度。
- 使用说明:
  - 底层源码内部以弧度保存姿态，但 Lua 接口返回前会自动换算成角度。
  - 传入无效部位名会报错 `invalid mannequin part`。

#### getRotationX(part)

- 语法: `getRotationX(part: string): number`
- 用途: 返回指定部位当前的 X 轴旋转角度。
- 参数:
  - `part`: 目标部位名。
- 返回值:
  - 一个角度值，单位为度。
- 使用说明:
  - 传入无效部位名会报错 `invalid mannequin part`。

#### getRotationY(part)

- 语法: `getRotationY(part: string): number`
- 用途: 返回指定部位当前的 Y 轴旋转角度。
- 参数:
  - `part`: 目标部位名。
- 返回值:
  - 正常情况下返回一个角度值，单位为度。
- 使用说明:
  - 传入无效部位名会报错 `invalid mannequin part`。

#### getRotationZ(part)

- 语法: `getRotationZ(part: string): number`
- 用途: 返回指定部位当前的 Z 轴旋转角度。
- 参数:
  - `part`: 目标部位名。
- 返回值:
  - 正常情况下返回一个角度值，单位为度。
- 使用说明:
  - 传入无效部位名会报错 `invalid mannequin part`。

#### setRotation(part[, x[, y[, z]]])

- 语法: `setRotation(part: string[, x: number | nil[, y: number | nil[, z: number | nil]]])`
- 用途: 一次调用同时修改一个部位的一个或多个旋转轴。
- 参数:
  - `part`: 目标部位名。
  - `x`: 新的 X 轴角度，传 `nil` 表示保持原值。
  - `y`: 新的 Y 轴角度，传 `nil` 表示保持原值。
  - `z`: 新的 Z 轴角度，传 `nil` 表示保持原值。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 每个实际传入的角度都必须位于 `-180` 到 `180` 之间。
  - 写入前会先把角度转换成弧度再保存到底层人偶数据。
  - 超出范围会报错 `rotation must be between -180 and 180`。
  - `setRotation("head", nil, 45, nil)` 是合法写法，只会修改 Y 轴。

#### setRotationX(part, x)

- 语法: `setRotationX(part: string, x: number)`
- 用途: 只修改指定部位的 X 轴旋转角度。
- 参数:
  - `part`: 目标部位名。
  - `x`: 新的 X 轴角度，单位为度。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 合法范围是 `-180` 到 `180`。
  - 超出范围会报错 `rotation must be between -180 and 180`。

#### setRotationY(part, y)

- 语法: `setRotationY(part: string, y: number)`
- 用途: 只修改指定部位的 Y 轴旋转角度。
- 参数:
  - `part`: 目标部位名。
  - `y`: 新的 Y 轴角度，单位为度。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 合法范围是 `-180` 到 `180`。
  - 超出范围会报错 `rotation must be between -180 and 180`。

#### setRotationZ(part, z)

- 语法: `setRotationZ(part: string, z: number)`
- 用途: 只修改指定部位的 Z 轴旋转角度。
- 参数:
  - `part`: 目标部位名。
  - `z`: 新的 Z 轴角度，单位为度。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 合法范围是 `-180` 到 `180`。
  - 超出范围会报错 `rotation must be between -180 and 180`。

#### parts

- 语法: `parts`
- 用途: 返回内置的全部有效部位名列表。
- 返回值:
  - 一个带索引的 Lua 表，内容依次为 `head`、`chest`、`left_arm`、`right_arm`、`left_leg`、`right_leg`。
- 使用说明:
  - 如果你想遍历全部可控制部位，用这个 getter 比手写字符串更稳妥。

## 示例

```lua
local component = require("component")
local mannequin = component.mannequin

for index, name in pairs(mannequin.parts) do
  print(index, name)
end

mannequin.setRotation("head", 0, 30, 0)
mannequin.setRotationX("left_arm", -45)
mannequin.setRotationZ("right_arm", 20)

local x, y, z = mannequin.getRotation("head")
print("Head rotation:", x, y, z)
```

## 相关内容

- `opencomputers-redstone-signaller`
