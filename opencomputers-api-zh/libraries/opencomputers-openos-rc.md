# rc

## 概述

供 `rc` 管理的启动脚本使用的服务注册表与运行级别辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\rc.lua`

## 用法

```lua
local rc = require("rc")
```

## 接口

## 备注

- 这个较小的引导模块只先建立 `rc.loaded`；其余辅助函数会在正常 OpenOS 风格系统加载延迟实现后补齐。
