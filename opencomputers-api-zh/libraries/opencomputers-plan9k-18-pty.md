# pty

## 概述

被 Plan9k 屏幕会话和 shell 进程使用的内部伪终端端点对象。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\18_pty.lua`

## 用法

```lua
-- internal pseudo-terminal object allocated by the Plan9k PTY manager
```

## 接口

### pty.read(...)

- 说明: 读取从终端主端到达、可供从端 PTY 端点消费的字节数据。
- 参数:
  - `...`

```lua
pty.read(nil)
```

### pty.write(...)

- 说明: 把来自从端 PTY 端点的数据写回终端主端方向。
- 参数:
  - `...`

```lua
pty.write(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
