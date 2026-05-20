# rc

## 概述

供 `rc` 管理的启动脚本使用的服务注册表与运行级别辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\rc.lua`

## 用法

```lua
local rc = require("rc")
```

## 接口

### rc.allRunCommand(cmd, ...)

- 说明: 对所有已加载的 `rc` 脚本执行同一个服务命令，并收集逐服务结果。
- 参数:
  - `cmd`
  - `...`

```lua
rc.allRunCommand(nil, nil)
```

### rc.load(name, args)

- 说明: 加载一个服务定义，使后续 `rc` 命令可以按名字访问它。
- 参数:
  - `name`
  - `args`

```lua
rc.load("value", nil)
```

### rc.runCommand(name, cmd, ...)

- 说明: 在指定已加载服务上调用诸如 `start` 或 `stop` 之类的命令。
- 参数:
  - `name`
  - `cmd`
  - `...`

```lua
rc.runCommand("value", nil, nil)
```

### rc.unload(name)

- 说明: 把先前加载的服务从内存注册表中移除。
- 参数:
  - `name`

```lua
rc.unload("value")
```

## 备注

- 这个较小的引导模块只先建立 `rc.loaded`；其余辅助函数会在正常 OpenOS 风格系统加载延迟实现后补齐。
