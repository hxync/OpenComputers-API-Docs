# disk_drive

## 概述

`disk_drive` 组件用于控制软盘驱动器。它的接口本身很小，主要提供三类能力：判断当前是否插着软盘、拿到已插入软盘内部文件系统的组件地址，以及把介质弹出到世界中。

独立方块版软驱和机架挂载式软驱使用的是同一套组件接口。

## 可用性

- 仓库：`opencomputers`
- 组件名：`disk_drive`
- 常见宿主：软盘驱动器方块、机架挂载式软驱

## 用法

```lua
local component = require("component")
local drive = component.disk_drive

if not drive then
  error("当前环境没有 disk_drive 组件")
end
```

## 接口

### isEmpty

- 语法：`drive.isEmpty()`
- 返回值：`boolean`
- 用途：判断当前驱动器里是否插着软盘。

返回值说明：

- `true` 表示驱动器为空；
- `false` 表示当前已经插入了一张软盘。

示例：

```lua
if drive.isEmpty() then
  print("请先插入软盘")
else
  print("当前已有软盘")
end
```

### eject

- 语法：`drive.eject([velocity])`
- 返回值：`boolean`
- 用途：把当前插入的软盘弹出到世界中。

参数说明：

- `velocity:number` 可选：弹出速度倍率。源码会把它限制到 `0..1`。

返回值说明：

- 有软盘且成功弹出时返回 `true`；
- 当前本来就是空驱动器时返回 `false`。

示例：

```lua
local ok = drive.eject(0.5)
print("是否已弹出:", ok)
```

### media

- 语法：`drive.media()`
- 返回值：
  - 成功：`string`
  - 失败：`nil, reason`
- 用途：返回当前插入软盘内部文件系统的组件地址。

源码中的失败原因是 `"drive is empty"`。

它通常用于从软驱桥接到普通文件系统组件：

```lua
local address, reason = drive.media()
if not address then
  print("当前没有软盘:", reason)
else
  local fs = component.proxy(address)
  for name in fs.list("/") do
    print(name)
  end
end
```

## 相关内容

- `filesystem`
- `component`
- `openos`
