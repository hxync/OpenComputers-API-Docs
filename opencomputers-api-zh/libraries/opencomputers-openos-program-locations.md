# programLocations

## 概述

把程序名映射到可制作软盘并输出缺失命令安装提示的辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\tools\programLocations.lua`

## 用法

```lua
local programLocations = dofile("/lib/tools/programLocations.lua")
```

## 接口

### programLocations.locate(path)

- 说明: 根据 `computer.getProgramLocations()` 提供的映射，查找某个程序路径对应的软盘或资源来源名。
- 参数:
  - `path`: 要查询的程序路径或程序名。

```lua
local loot = programLocations.locate('oppm')
```

### programLocations.reportNotFound(path, reason)

- 说明: 输出面向用户的未找到提示；如果缺失命令可通过可制作软盘安装，还会附带安装说明。
- 参数:
  - `path`: 解析失败的命令路径或程序名。
  - `reason`: 可选，底层解析过程返回的错误文本。

```lua
return programLocations.reportNotFound('oppm', reason)
```

## 备注

- 本页只列出模块导出的公开函数。
