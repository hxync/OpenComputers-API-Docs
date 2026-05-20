# procfs

## 概述

供 Plan9k `/proc` 文件系统实现使用的内部伪文件句柄辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\10_procfs.lua`

## 用法

```lua
-- internal procfs handle helper mounted under /proc
```

## 接口

### procfs.read(n)

- 说明: 读取当前打开句柄所对应的合成 procfs 文件数据片段。
- 参数:
  - `n`: 可选，本次从当前句柄位置最多读取的字节数。

```lua
local chunk = procfs.read(128)
```

## 备注

- 本页只列出模块导出的公开函数。
