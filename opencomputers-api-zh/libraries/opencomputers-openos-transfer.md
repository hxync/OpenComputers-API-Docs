# transfer

## 概述

被 `cp`、`mv`、`install` 等命令共享使用的递归复制与移动辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\tools\transfer.lua`

## 用法

```lua
local transfer = dofile("/lib/tools/transfer.lua")
```

## 接口

### transfer.batch(args, options)

- 说明: 把一次复制或移动操作应用到整组参数上，并把最后一个参数当作目标路径。
- 参数:
  - `args`: 源与目标组成的参数数组；最后一个元素是目标路径。
  - `options`: 规范化后的选项表，例如 `{cmd='cp', r=true, v=true}`。

```lua
local exitCode = transfer.batch({'src', 'dst'}, {cmd='cp', r=true})
```

### transfer.recurse(fromPath, toPath, options, origin, top)

- 说明: 把一个路径复制或移动到另一个路径中，并在进入目录时遵守覆盖、链接和挂载规则。
- 参数:
  - `fromPath`: 调用方写入的源路径。
  - `toPath`: 调用方写入的目标路径。
  - `options`: 在整个传输任务中共享的操作选项表。
  - `origin`: 挂载来源查找表，用于保护挂载点并识别跨挂载遍历。
  - `top`: 当前调用是否为顶层入口，而不是递归子调用。

```lua
transfer.recurse('from', 'to', {cmd='mv', v=true}, mountOrigins, true)
```

## 备注

- 这个辅助库理解目录递归、符号链接、挂载点保护、交互式覆盖模式，以及 shell 解析器构建出的标准化选项表。
