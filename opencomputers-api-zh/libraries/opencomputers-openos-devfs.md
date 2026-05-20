# devfs

## 概述

用于构建并暴露 OpenOS `/dev` 设备树的虚拟设备文件系统辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\devfs.lua`

## 用法

```lua
local devfs = require("devfs")
```

## 接口

### devfs.create(path, proxy)

- 说明: 在 devfs 树中创建一个虚拟设备节点或目录项。
- 参数:
  - `path`: 相对于 devfs 根目录的虚拟路径，例如 `/components/by-type`。
  - `proxy`: 新节点对应的代理表；传 `nil` 时创建一个空目录容器。

```lua
local node, reason = devfs.create("/custom/sensor", {read = function() return "42" end})
```

### devfs.getDevice(path)

- 说明: 把一个路径反解回底层设备代理；无论它指向 `/dev/...` 还是某个已挂载的文件系统条目都可以处理。
- 参数:
  - `path`: 要检查的绝对路径，或会先经过 shell 解析的路径。

```lua
local proxy, reason = devfs.getDevice("/dev/components/by-type/gpu")
```

### devfs.register(public_proxy)

- 说明: 从 `/lib/core/devfs` 加载启动模块，只执行一次地填充设备树，并把动态子节点发现逻辑挂到已挂载的文件系统代理上。
- 参数:
  - `public_proxy`: 已经挂载到 `/dev` 的文件系统代理表；如果可用，会向其中注入动态目录子节点。

```lua
devfs.register(fsProxy)
```

## 备注

- 这个公开模块主要负责创建设备节点，以及把设备路径反解回组件代理或文件系统代理。
- 像 `api.proxy.open` 这样的内部文件系统代理方法是挂载 `/dev` 时使用的实现细节，因此没有列在这里。
