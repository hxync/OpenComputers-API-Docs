# package

## 概述

用于模块加载状态管理、搜索路径解析和完整模块延迟加载的辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\package.lua`

## 用法

```lua
local package = require("package")
```

## 接口

### package.delay(lib, file)

- 说明: 挂接延迟加载元表，使更大的模块文件只在首次访问时才真正执行。
- 参数:
  - `lib`
  - `file`

```lua
package.delay(nil, nil)
```

### package.searchpath(name, path, sep, rep)

- 说明: 把模块名套入以分号分隔的 Lua 包路径模板列表，并解析出真实文件路径。
- 参数:
  - `name`
  - `path`
  - `sep`
  - `rep`

```lua
package.searchpath("value", nil, nil, nil)
```

## 备注

- 本页只列出模块导出的公开函数。
