# sh

## 概述

叠加在基础 `sh` 模块上的高级 shell 补全与文件/程序查找辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_sh.lua`

## 用法

```lua
local sh = require("sh")  -- advanced parsing helpers load lazily
```

## 接口

### sh.getMatchingFiles(partial_path)

- 说明: 为一个部分输入的路径返回文件系统补全候选。
- 参数:
  - `partial_path`: 用户原样输入的部分路径；它可以是相对路径，也可以包含转义。

```lua
local files = sh.getMatchingFiles('/et')
```

### sh.getMatchingPrograms(baseName)

- 说明: 从 shell 别名以及 `PATH` 中的每个目录里返回命令名补全候选。
- 参数:
  - `baseName`: 用户已经输入的前缀。

```lua
local programs = sh.getMatchingPrograms('gr')
```

## 备注

- 本页只列出模块导出的公开函数。
