# os

## 概述

OpenComputers 会覆盖部分 Lua `os` 表行为，使其反映游戏内时间语义，而不是宿主操作系统时间。

## 可用性

- 仓库: `opencomputers`
- 类型: 核心运行时接口

## 来源

- `OpenComputers/src\main\scala\li\cil\oc\server\machine\luac\OSAPI.scala`

## 用法

```lua
print(os.clock())
print(os.date())
```

## 接口

### clock()

- 说明: 返回机器已消耗的 CPU 时间。

### date([format[, time]])

- 说明: 格式化游戏内时间戳，或返回日期表。

### time([table])

- 说明: 返回当前游戏内时间戳，或转换日期表。

## 相关内容

- `computer`
- `component`
- `os`
- `unicode`
